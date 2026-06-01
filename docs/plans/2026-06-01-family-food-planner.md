# Family Food Planner — Implementation Plan

> **For Hermes:** Use subagent-driven-development skill to implement this plan task-by-task.

**Goal:** Build an AI-powered meal planning & grocery management web app that eliminates the daily "what's for dinner?" stress.

**Architecture:** Next.js 15 App Router with Prisma + SQLite (local-first). Single-page meal planner grid, recipe management, and auto-generated grocery lists. No auth for MVP — single family instance. Mobile-first responsive design for grocery list on-the-go.

**Tech Stack:** Next.js 15 (App Router) • TypeScript • Prisma 7 + SQLite • shadcn/ui (base-ui) • Tailwind CSS • lucide-react icons

---

## Phase 1: Project Scaffold & Database

### Task 1: Create Next.js project

**Objective:** Scaffold the Next.js app with TypeScript, Tailwind, and App Router.

**Files:**
- Create: `family-food-planner/` (new project directory)

**Step 1: Create the app**

```bash
cd /home/server
npx create-next-app@latest family-food-planner --typescript --tailwind --eslint --app --src-dir --import-alias "@/*" --use-npm --no-turbopack
```

**Step 2: Install dependencies**

```bash
cd /home/server/family-food-planner
npm install --legacy-peer-deps @prisma/client prisma
npm install --legacy-peer-deps lucide-react clsx tailwind-merge class-variance-authority
```

**Step 3: Initialize Prisma with SQLite**

```bash
npx prisma init --datasource-provider sqlite
```

**Step 4: Initialize shadcn/ui**

```bash
npx shadcn@latest init -d -y
```

**Step 5: Add shadcn components**

```bash
npx shadcn@latest add card button input label dialog select separator tabs badge sheet scroll-area textarea sonner -y --overwrite
```

**Step 6: Verify build**

```bash
npm run build
```

Expected: Build succeeds with no errors.

**Step 7: Commit**

```bash
git init
git add .
git commit -m "feat: scaffold Next.js + Prisma + shadcn/ui project"
```

---

### Task 2: Define Prisma schema

**Objective:** Create the database models for meals, recipes, ingredients, and grocery lists.

**Files:**
- Modify: `prisma/schema.prisma`
- Create: `src/lib/prisma.ts`

**Step 1: Write the schema**

```prisma
// prisma/schema.prisma
generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "sqlite"
  url      = "file:./dev.db"
}

model Recipe {
  id          String   @id @default(cuid())
  name        String
  mealType    String   // "breakfast" | "lunch" | "dinner"
  description String?
  servings    Int      @default(4)
  prepTime    Int?     // minutes
  notes       String?
  isFavorite  Boolean  @default(false)
  createdAt   DateTime @default(now())
  updatedAt   DateTime @updatedAt

  ingredients Ingredient[]
  mealSlots   MealSlot[]
}

model Ingredient {
  id       String @id @default(cuid())
  name     String
  quantity Float
  unit     String   // "pcs", "g", "ml", "cup", "tbsp", etc.
  recipeId String

  recipe   Recipe @relation(fields: [recipeId], references: [id], onDelete: Cascade)
}

model MealSlot {
  id       String @id @default(cuid())
  day      Int    // 0-9 (10-day rotation)
  mealType String // "breakfast" | "lunch" | "dinner"
  recipeId String

  recipe   Recipe @relation(fields: [recipeId], references: [id], onDelete: Cascade)

  @@unique([day, mealType])
}

model GroceryItem {
  id        String  @id @default(cuid())
  name      String
  quantity  Float
  unit      String
  checked   Boolean @default(false)
  source    String? // recipe name that generated it
  createdAt DateTime @default(now())

  @@unique([name, unit]) // deduplicate same items
}
```

**Step 2: Create Prisma client singleton**

```typescript
// src/lib/prisma.ts
import { PrismaClient } from "@prisma/client";

const globalForPrisma = globalThis as unknown as { prisma: PrismaClient };

export const prisma = globalForPrisma.prisma || new PrismaClient();

if (process.env.NODE_ENV !== "production") globalForPrisma.prisma = prisma;
```

**Step 3: Push schema to database**

```bash
npx prisma generate
npx prisma db push
```

Expected: `The database is now in sync with the Prisma schema.`

**Step 4: Verify with Prisma Studio**

```bash
npx prisma studio
```

Expected: Opens browser at http://localhost:5555 showing empty tables.

**Step 5: Commit**

```bash
git add prisma/schema.prisma src/lib/prisma.ts
git commit -m "feat: add Prisma schema — recipes, ingredients, meal slots, grocery items"
```

---

## Phase 2: Recipe Management

### Task 3: Recipe list page

**Objective:** Display all recipes in a card grid with meal type badges.

**Files:**
- Create: `src/app/page.tsx` (replace default)
- Create: `src/app/layout.tsx` (modify for app layout)
- Create: `src/components/recipe-card.tsx`
- Create: `src/app/api/recipes/route.ts`

**Step 1: Create the recipes API route**

```typescript
// src/app/api/recipes/route.ts
import { NextResponse } from "next/server";
import { prisma } from "@/lib/prisma";

export const dynamic = "force-dynamic";

export async function GET() {
  const recipes = await prisma.recipe.findMany({
    include: { ingredients: true },
    orderBy: { updatedAt: "desc" },
  });
  return NextResponse.json(recipes);
}

export async function POST(req: Request) {
  const body = await req.json();
  const recipe = await prisma.recipe.create({
    data: {
      name: body.name,
      mealType: body.mealType,
      description: body.description,
      servings: body.servings ?? 4,
      prepTime: body.prepTime,
      notes: body.notes,
      ingredients: {
        create: body.ingredients ?? [],
      },
    },
    include: { ingredients: true },
  });
  return NextResponse.json(recipe);
}
```

**Step 2: Create RecipeCard component**

```tsx
// src/components/recipe-card.tsx
"use client";

import { Card, CardContent, CardHeader, CardTitle } from "@/components/ui/card";
import { Badge } from "@/components/ui/badge";
import { Clock, Users } from "lucide-react";

interface Recipe {
  id: string;
  name: string;
  mealType: string;
  description: string | null;
  servings: number;
  prepTime: number | null;
  ingredients: { id: string; name: string; quantity: number; unit: string }[];
}

const mealColors: Record<string, string> = {
  breakfast: "bg-orange-500",
  lunch: "bg-blue-500",
  dinner: "bg-purple-500",
};

export function RecipeCard({ recipe }: { recipe: Recipe }) {
  return (
    <Card className="hover:shadow-lg transition-shadow cursor-pointer">
      <CardHeader className="pb-2">
        <div className="flex justify-between items-start">
          <CardTitle className="text-lg">{recipe.name}</CardTitle>
          <Badge className={mealColors[recipe.mealType] ?? "bg-gray-500"}>
            {recipe.mealType}
          </Badge>
        </div>
        {recipe.description && (
          <p className="text-sm text-muted-foreground">{recipe.description}</p>
        )}
      </CardHeader>
      <CardContent>
        <div className="flex gap-4 text-sm text-muted-foreground">
          {recipe.prepTime && (
            <span className="flex items-center gap-1">
              <Clock className="h-4 w-4" /> {recipe.prepTime}m
            </span>
          )}
          <span className="flex items-center gap-1">
            <Users className="h-4 w-4" /> {recipe.servings}
          </span>
          <span>{recipe.ingredients.length} ingredients</span>
        </div>
      </CardContent>
    </Card>
  );
}
```

**Step 3: Create the home page**

```tsx
// src/app/page.tsx
import { prisma } from "@/lib/prisma";
import { RecipeCard } from "@/components/recipe-card";
import Link from "next/link";
import { Button } from "@/components/ui/button";
import { Plus } from "lucide-react";

export const dynamic = "force-dynamic";

export default async function HomePage() {
  const recipes = await prisma.recipe.findMany({
    include: { ingredients: true },
    orderBy: { updatedAt: "desc" },
  });

  return (
    <main className="container mx-auto p-4 max-w-6xl">
      <div className="flex justify-between items-center mb-6">
        <div>
          <h1 className="text-3xl font-bold">🍽️ Family Food Planner</h1>
          <p className="text-muted-foreground">Plan meals, track groceries, feed the family.</p>
        </div>
        <Link href="/recipes/new">
          <Button><Plus className="h-4 w-4 mr-2" /> Add Recipe</Button>
        </Link>
      </div>

      {recipes.length === 0 ? (
        <div className="text-center py-20 text-muted-foreground">
          <p className="text-xl mb-2">No recipes yet</p>
          <p>Add your first recipe to get started.</p>
        </div>
      ) : (
        <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">
          {recipes.map((recipe) => (
            <Link key={recipe.id} href={`/recipes/${recipe.id}`}>
              <RecipeCard recipe={recipe} />
            </Link>
          ))}
        </div>
      )}
    </main>
  );
}
```

**Step 4: Build and verify**

```bash
npm run build
```

Expected: Build succeeds. Home page shows empty state.

**Step 5: Commit**

```bash
git add .
git commit -m "feat: recipe list page with card grid and API routes"
```

---

### Task 4: Add recipe form

**Objective:** Create a form to add new recipes with dynamic ingredient fields.

**Files:**
- Create: `src/app/recipes/new/page.tsx`
- Create: `src/components/recipe-form.tsx`

**Step 1: Create the recipe form component**

```tsx
// src/components/recipe-form.tsx
"use client";

import { useState } from "react";
import { useRouter } from "next/navigation";
import { Button } from "@/components/ui/button";
import { Input } from "@/components/ui/input";
import { Label } from "@/components/ui/label";
import { Textarea } from "@/components/ui/textarea";
import { Select, SelectContent, SelectItem, SelectTrigger, SelectValue } from "@/components/ui/select";
import { Plus, Trash2 } from "lucide-react";
import { toast } from "sonner";

interface IngredientInput {
  name: string;
  quantity: string;
  unit: string;
}

export function RecipeForm() {
  const router = useRouter();
  const [saving, setSaving] = useState(false);
  const [ingredients, setIngredients] = useState<IngredientInput[]>([
    { name: "", quantity: "", unit: "pcs" },
  ]);

  function addIngredient() {
    setIngredients([...ingredients, { name: "", quantity: "", unit: "pcs" }]);
  }

  function removeIngredient(i: number) {
    setIngredients(ingredients.filter((_, idx) => idx !== i));
  }

  function updateIngredient(i: number, field: keyof IngredientInput, value: string) {
    const updated = [...ingredients];
    updated[i][field] = value;
    setIngredients(updated);
  }

  async function handleSubmit(e: React.FormEvent<HTMLFormElement>) {
    e.preventDefault();
    setSaving(true);

    const form = new FormData(e.currentTarget);
    const data = {
      name: form.get("name") as string,
      mealType: form.get("mealType") as string,
      description: form.get("description") as string || null,
      servings: parseInt(form.get("servings") as string) || 4,
      prepTime: parseInt(form.get("prepTime") as string) || null,
      notes: form.get("notes") as string || null,
      ingredients: ingredients
        .filter((ing) => ing.name.trim())
        .map((ing) => ({
          name: ing.name.trim(),
          quantity: parseFloat(ing.quantity) || 1,
          unit: ing.unit,
        })),
    };

    const res = await fetch("/api/recipes", {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify(data),
    });

    if (res.ok) {
      toast.success("Recipe added!");
      router.push("/");
      router.refresh();
    } else {
      toast.error("Failed to add recipe");
    }
    setSaving(false);
  }

  return (
    <form onSubmit={handleSubmit} className="space-y-6 max-w-2xl">
      <div className="space-y-2">
        <Label htmlFor="name">Recipe Name</Label>
        <Input id="name" name="name" required placeholder="e.g., Pancakes" />
      </div>

      <div className="grid grid-cols-3 gap-4">
        <div className="space-y-2">
          <Label htmlFor="mealType">Meal Type</Label>
          <Select name="mealType" required>
            <SelectTrigger><SelectValue placeholder="Select" /></SelectTrigger>
            <SelectContent>
              <SelectItem value="breakfast">Breakfast</SelectItem>
              <SelectItem value="lunch">Lunch</SelectItem>
              <SelectItem value="dinner">Dinner</SelectItem>
            </SelectContent>
          </Select>
        </div>
        <div className="space-y-2">
          <Label htmlFor="servings">Servings</Label>
          <Input id="servings" name="servings" type="number" defaultValue={4} min={1} />
        </div>
        <div className="space-y-2">
          <Label htmlFor="prepTime">Prep (min)</Label>
          <Input id="prepTime" name="prepTime" type="number" placeholder="30" />
        </div>
      </div>

      <div className="space-y-2">
        <Label htmlFor="description">Description</Label>
        <Textarea id="description" name="description" placeholder="Quick description..." rows={2} />
      </div>

      <div className="space-y-2">
        <div className="flex justify-between items-center">
          <Label>Ingredients</Label>
          <Button type="button" variant="outline" size="sm" onClick={addIngredient}>
            <Plus className="h-4 w-4 mr-1" /> Add
          </Button>
        </div>
        {ingredients.map((ing, i) => (
          <div key={i} className="flex gap-2 items-center">
            <Input
              placeholder="Ingredient name"
              value={ing.name}
              onChange={(e) => updateIngredient(i, "name", e.target.value)}
              className="flex-1"
            />
            <Input
              placeholder="Qty"
              type="number"
              value={ing.quantity}
              onChange={(e) => updateIngredient(i, "quantity", e.target.value)}
              className="w-20"
              step="0.1"
            />
            <Select value={ing.unit} onValueChange={(v) => updateIngredient(i, "unit", v)}>
              <SelectTrigger className="w-24"><SelectValue /></SelectTrigger>
              <SelectContent>
                <SelectItem value="pcs">pcs</SelectItem>
                <SelectItem value="g">g</SelectItem>
                <SelectItem value="ml">ml</SelectItem>
                <SelectItem value="cup">cup</SelectItem>
                <SelectItem value="tbsp">tbsp</SelectItem>
                <SelectItem value="tsp">tsp</SelectItem>
                <SelectItem value="l">l</SelectItem>
                <SelectItem value="kg">kg</SelectItem>
              </SelectContent>
            </Select>
            <Button type="button" variant="ghost" size="icon" onClick={() => removeIngredient(i)}>
              <Trash2 className="h-4 w-4 text-destructive" />
            </Button>
          </div>
        ))}
      </div>

      <div className="space-y-2">
        <Label htmlFor="notes">Notes</Label>
        <Textarea id="notes" name="notes" placeholder="Any tips or notes..." rows={2} />
      </div>

      <div className="flex gap-3">
        <Button type="submit" disabled={saving}>{saving ? "Saving..." : "Save Recipe"}</Button>
        <Button type="button" variant="outline" onClick={() => router.back()}>Cancel</Button>
      </div>
    </form>
  );
}
```

**Step 2: Create the new recipe page**

```tsx
// src/app/recipes/new/page.tsx
import { RecipeForm } from "@/components/recipe-form";

export default function NewRecipePage() {
  return (
    <main className="container mx-auto p-4 max-w-6xl">
      <h1 className="text-2xl font-bold mb-6">Add New Recipe</h1>
      <RecipeForm />
    </main>
  );
}
```

**Step 3: Build and verify**

```bash
npm run build
```

**Step 4: Commit**

```bash
git add .
git commit -m "feat: add recipe form with dynamic ingredient fields"
```

---

## Phase 3: Meal Planner Grid

### Task 5: Meal planner page with 10-day grid

**Objective:** Display a 10-day × 3-meal grid where users can assign recipes to slots.

**Files:**
- Create: `src/app/planner/page.tsx`
- Create: `src/components/meal-planner-grid.tsx`
- Create: `src/components/meal-slot-picker.tsx`
- Create: `src/app/api/planner/route.ts`

**Step 1: Create planner API routes**

```typescript
// src/app/api/planner/route.ts
import { NextResponse } from "next/server";
import { prisma } from "@/lib/prisma";

export const dynamic = "force-dynamic";

export async function GET() {
  const slots = await prisma.mealSlot.findMany({
    include: { recipe: { include: { ingredients: true } } },
    orderBy: [{ day: "asc" }, { mealType: "asc" }],
  });
  return NextResponse.json(slots);
}

export async function POST(req: Request) {
  const body = await req.json();
  const slot = await prisma.mealSlot.upsert({
    where: { day_mealType: { day: body.day, mealType: body.mealType } },
    update: { recipeId: body.recipeId },
    create: { day: body.day, mealType: body.mealType, recipeId: body.recipeId },
    include: { recipe: true },
  });
  return NextResponse.json(slot);
}

export async function DELETE(req: Request) {
  const body = await req.json();
  await prisma.mealSlot.delete({
    where: { day_mealType: { day: body.day, mealType: body.mealType } },
  });
  return NextResponse.json({ ok: true });
}
```

**Step 2: Create MealSlotPicker component**

```tsx
// src/components/meal-slot-picker.tsx
"use client";

import { Select, SelectContent, SelectItem, SelectTrigger, SelectValue } from "@/components/ui/select";
import { toast } from "sonner";

interface Recipe {
  id: string;
  name: string;
}

interface Props {
  day: number;
  mealType: string;
  currentRecipeId: string | null;
  recipes: Recipe[];
  onAssigned: () => void;
}

export function MealSlotPicker({ day, mealType, currentRecipeId, recipes, onAssigned }: Props) {
  async function handleChange(recipeId: string) {
    if (recipeId === "__clear") {
      await fetch("/api/planner", {
        method: "DELETE",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ day, mealType }),
      });
    } else {
      await fetch("/api/planner", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ day, mealType, recipeId }),
      });
    }
    toast.success("Meal updated");
    onAssigned();
  }

  return (
    <Select value={currentRecipeId ?? ""} onValueChange={handleChange}>
      <SelectTrigger className="w-full text-sm">
        <SelectValue placeholder="Pick a meal" />
      </SelectTrigger>
      <SelectContent>
        {currentRecipeId && (
          <SelectItem value="__clear">✕ Remove</SelectItem>
        )}
        {recipes.map((r) => (
          <SelectItem key={r.id} value={r.id}>{r.name}</SelectItem>
        ))}
      </SelectContent>
    </Select>
  );
}
```

**Step 3: Create MealPlannerGrid component**

```tsx
// src/components/meal-planner-grid.tsx
"use client";

import { useState, useEffect, useCallback } from "react";
import { Card, CardContent, CardHeader, CardTitle } from "@/components/ui/card";
import { MealSlotPicker } from "./meal-slot-picker";
import { Sun, Coffee, Moon } from "lucide-react";

const DAYS = ["Day 1", "Day 2", "Day 3", "Day 4", "Day 5", "Day 6", "Day 7", "Day 8", "Day 9", "Day 10"];
const MEALS = [
  { type: "breakfast", label: "Breakfast", icon: Coffee },
  { type: "lunch", label: "Lunch", icon: Sun },
  { type: "dinner", label: "Dinner", icon: Moon },
];

interface Recipe { id: string; name: string; }
interface Slot { day: number; mealType: string; recipeId: string; recipe: Recipe; }

export function MealPlannerGrid() {
  const [slots, setSlots] = useState<Slot[]>([]);
  const [recipes, setRecipes] = useState<Recipe[]>([]);

  const refresh = useCallback(async () => {
    const [slotsRes, recipesRes] = await Promise.all([
      fetch("/api/planner").then((r) => r.json()),
      fetch("/api/recipes").then((r) => r.json()),
    ]);
    setSlots(slotsRes);
    setRecipes(recipesRes);
  }, []);

  useEffect(() => { refresh(); }, [refresh]);

  function getSlot(day: number, mealType: string) {
    return slots.find((s) => s.day === day && s.mealType === mealType);
  }

  return (
    <div className="overflow-x-auto">
      <table className="w-full border-collapse">
        <thead>
          <tr>
            <th className="p-2 text-left text-sm font-medium text-muted-foreground w-24"></th>
            {DAYS.map((day) => (
              <th key={day} className="p-2 text-center text-sm font-medium">{day}</th>
            ))}
          </tr>
        </thead>
        <tbody>
          {MEALS.map(({ type, label, icon: Icon }) => (
            <tr key={type} className="border-t">
              <td className="p-2">
                <div className="flex items-center gap-2 text-sm font-medium">
                  <Icon className="h-4 w-4" /> {label}
                </div>
              </td>
              {DAYS.map((_, i) => {
                const slot = getSlot(i, type);
                return (
                  <td key={i} className="p-1">
                    <MealSlotPicker
                      day={i}
                      mealType={type}
                      currentRecipeId={slot?.recipeId ?? null}
                      recipes={recipes}
                      onAssigned={refresh}
                    />
                  </td>
                );
              })}
            </tr>
          ))}
        </tbody>
      </table>
    </div>
  );
}
```

**Step 4: Create the planner page**

```tsx
// src/app/planner/page.tsx
import { MealPlannerGrid } from "@/components/meal-planner-grid";

export default function PlannerPage() {
  return (
    <main className="container mx-auto p-4 max-w-7xl">
      <h1 className="text-2xl font-bold mb-2">📅 10-Day Meal Planner</h1>
      <p className="text-muted-foreground mb-6">Assign recipes to each meal slot. The rotation repeats every 10 days.</p>
      <MealPlannerGrid />
    </main>
  );
}
```

**Step 5: Build and verify**

```bash
npm run build
```

**Step 6: Commit**

```bash
git add .
git commit -m "feat: 10-day meal planner grid with slot assignment"
```

---

## Phase 4: Grocery List Generator

### Task 6: Auto-generate grocery list from meal plan

**Objective:** Aggregate all ingredients from scheduled meals into a deduplicated grocery list.

**Files:**
- Create: `src/app/grocery/page.tsx`
- Create: `src/components/grocery-list.tsx`
- Create: `src/app/api/grocery/route.ts`
- Create: `src/lib/grocery-utils.ts`

**Step 1: Create grocery utility**

```typescript
// src/lib/grocery-utils.ts
interface Ingredient {
  name: string;
  quantity: number;
  unit: string;
  recipeName: string;
}

interface GroceryEntry {
  name: string;
  quantity: number;
  unit: string;
  sources: string[];
}

export function aggregateIngredients(ingredients: Ingredient[]): GroceryEntry[] {
  const map = new Map<string, GroceryEntry>();

  for (const ing of ingredients) {
    const key = `${ing.name.toLowerCase()}|${ing.unit}`;
    const existing = map.get(key);
    if (existing) {
      existing.quantity += ing.quantity;
      if (!existing.sources.includes(ing.recipeName)) {
        existing.sources.push(ing.recipeName);
      }
    } else {
      map.set(key, {
        name: ing.name,
        quantity: ing.quantity,
        unit: ing.unit,
        sources: [ing.recipeName],
      });
    }
  }

  return Array.from(map.values()).sort((a, b) => a.name.localeCompare(b.name));
}
```

**Step 2: Create grocery API**

```typescript
// src/app/api/grocery/route.ts
import { NextResponse } from "next/server";
import { prisma } from "@/lib/prisma";
import { aggregateIngredients } from "@/lib/grocery-utils";

export const dynamic = "force-dynamic";

// Generate grocery list from current meal plan
export async function GET() {
  const slots = await prisma.mealSlot.findMany({
    include: { recipe: { include: { ingredients: true } } },
  });

  const allIngredients = slots.flatMap((slot) =>
    slot.recipe.ingredients.map((ing) => ({
      name: ing.name,
      quantity: ing.quantity,
      unit: ing.unit,
      recipeName: slot.recipe.name,
    }))
  );

  const groceryList = aggregateIngredients(allIngredients);
  return NextResponse.json(groceryList);
}

// Save/update grocery items (checked state)
export async function POST(req: Request) {
  const body = await req.json();
  const item = await prisma.groceryItem.upsert({
    where: { name_unit: { name: body.name, unit: body.unit } },
    update: { checked: body.checked, quantity: body.quantity, source: body.source },
    create: { name: body.name, unit: body.unit, quantity: body.quantity, checked: body.checked, source: body.source },
  });
  return NextResponse.json(item);
}

// Get saved grocery items (with checked state)
export async function PUT() {
  const items = await prisma.groceryItem.findMany({
    orderBy: [{ checked: "asc" }, { name: "asc" }],
  });
  return NextResponse.json(items);
}
```

**Step 3: Create GroceryList component**

```tsx
// src/components/grocery-list.tsx
"use client";

import { useState, useEffect, useCallback } from "react";
import { Button } from "@/components/ui/button";
import { Checkbox } from "@/components/ui/checkbox";
import { RefreshCw, Copy, Trash2 } from "lucide-react";
import { toast } from "sonner";

interface GroceryEntry {
  name: string;
  quantity: number;
  unit: string;
  sources: string[];
}

interface SavedItem extends GroceryEntry {
  id: string;
  checked: boolean;
}

export function GroceryList() {
  const [generated, setGenerated] = useState<GroceryEntry[]>([]);
  const [checked, setChecked] = useState<Set<string>>(new Set());
  const [loading, setLoading] = useState(false);

  const refresh = useCallback(async () => {
    setLoading(true);
    const res = await fetch("/api/grocery");
    const data = await res.json();
    setGenerated(data);
    setLoading(false);
  }, []);

  useEffect(() => { refresh(); }, [refresh]);

  function toggle(name: string, unit: string) {
    const key = `${name}|${unit}`;
    setChecked((prev) => {
      const next = new Set(prev);
      if (next.has(key)) next.delete(key);
      else next.add(key);
      return next;
    });
  }

  function copyList() {
    const unchecked = generated.filter((g) => !checked.has(`${g.name}|${g.unit}`));
    const text = unchecked.map((g) => `${g.quantity} ${g.unit} ${g.name}`).join("\n");
    navigator.clipboard.writeText(text);
    toast.success("Copied to clipboard!");
  }

  async function clearChecked() {
    setChecked(new Set());
    toast.success("Checked items cleared");
  }

  const uncheckedCount = generated.filter((g) => !checked.has(`${g.name}|${g.unit}`)).length;

  return (
    <div>
      <div className="flex gap-2 mb-4">
        <Button variant="outline" size="sm" onClick={refresh} disabled={loading}>
          <RefreshCw className="h-4 w-4 mr-1" /> Regenerate
        </Button>
        <Button variant="outline" size="sm" onClick={copyList}>
          <Copy className="h-4 w-4 mr-1" /> Copy List
        </Button>
        {checked.size > 0 && (
          <Button variant="outline" size="sm" onClick={clearChecked}>
            <Trash2 className="h-4 w-4 mr-1" /> Clear Checked
          </Button>
        )}
      </div>

      {generated.length === 0 ? (
        <p className="text-muted-foreground py-8 text-center">
          No meals planned yet. Add meals to the planner first.
        </p>
      ) : (
        <div className="space-y-1">
          <p className="text-sm text-muted-foreground mb-3">
            {uncheckedCount} items remaining
          </p>
          {generated.map((item) => {
            const key = `${item.name}|${item.unit}`;
            const isChecked = checked.has(key);
            return (
              <div
                key={key}
                className={`flex items-center gap-3 p-2 rounded ${isChecked ? "opacity-50 line-through" : ""}`}
              >
                <Checkbox checked={isChecked} onCheckedChange={() => toggle(item.name, item.unit)} />
                <span className="flex-1">{item.name}</span>
                <span className="text-sm text-muted-foreground w-20 text-right">
                  {item.quantity} {item.unit}
                </span>
              </div>
            );
          })}
        </div>
      )}
    </div>
  );
}
```

**Step 4: Create grocery page**

```tsx
// src/app/grocery/page.tsx
import { GroceryList } from "@/components/grocery-list";

export default function GroceryPage() {
  return (
    <main className="container mx-auto p-4 max-w-3xl">
      <h1 className="text-2xl font-bold mb-2">🛒 Grocery List</h1>
      <p className="text-muted-foreground mb-6">Auto-generated from your meal plan. Check off items as you shop.</p>
      <GroceryList />
    </main>
  );
}
```

**Step 5: Build and verify**

```bash
npm run build
```

**Step 6: Commit**

```bash
git add .
git commit -m "feat: grocery list auto-generated from meal plan with checkoff"
```

---

## Phase 5: Navigation & Polish

### Task 7: Add navigation bar

**Objective:** Add a responsive nav bar linking Home, Planner, and Grocery.

**Files:**
- Create: `src/components/nav.tsx`
- Modify: `src/app/layout.tsx`

**Step 1: Create Nav component**

```tsx
// src/components/nav.tsx
"use client";

import Link from "next/link";
import { usePathname } from "next/navigation";
import { Button } from "@/components/ui/button";
import { UtensilsCrossed, CalendarDays, ShoppingCart } from "lucide-react";
import { cn } from "@/lib/utils";

const links = [
  { href: "/", label: "Recipes", icon: UtensilsCrossed },
  { href: "/planner", label: "Planner", icon: CalendarDays },
  { href: "/grocery", label: "Grocery", icon: ShoppingCart },
];

export function Nav() {
  const pathname = usePathname();

  return (
    <nav className="border-b">
      <div className="container mx-auto max-w-7xl flex gap-1 p-2">
        {links.map(({ href, label, icon: Icon }) => (
          <Link key={href} href={href}>
            <Button
              variant={pathname === href ? "secondary" : "ghost"}
              className={cn("gap-2")}
            >
              <Icon className="h-4 w-4" /> {label}
            </Button>
          </Link>
        ))}
      </div>
    </nav>
  );
}
```

**Step 2: Update root layout**

```tsx
// src/app/layout.tsx
import type { Metadata } from "next";
import { Inter } from "next/font/google";
import "./globals.css";
import { Nav } from "@/components/nav";
import { Toaster } from "@/components/ui/sonner";

const inter = Inter({ subsets: ["latin"] });

export const metadata: Metadata = {
  title: "Family Food Planner",
  description: "AI-powered meal planning & grocery management",
};

export default function RootLayout({ children }: { children: React.ReactNode }) {
  return (
    <html lang="en">
      <body className={inter.className}>
        <Nav />
        {children}
        <Toaster />
      </body>
    </html>
  );
}
```

**Step 3: Build and verify**

```bash
npm run build
```

**Step 4: Commit**

```bash
git add .
git commit -m "feat: add navigation bar with Recipes, Planner, Grocery links"
```

---

### Task 8: Seed sample recipes

**Objective:** Add a seed script with 5-10 sample recipes so the app isn't empty on first run.

**Files:**
- Create: `prisma/seed.ts`

**Step 1: Create seed script**

```typescript
// prisma/seed.ts
import { PrismaClient } from "@prisma/client";
const prisma = new PrismaClient();

async function main() {
  const recipes = [
    {
      name: "Pancakes",
      mealType: "breakfast",
      description: "Fluffy pancakes with maple syrup",
      servings: 4,
      prepTime: 20,
      ingredients: [
        { name: "Flour", quantity: 200, unit: "g" },
        { name: "Eggs", quantity: 2, unit: "pcs" },
        { name: "Milk", quantity: 300, unit: "ml" },
        { name: "Butter", quantity: 30, unit: "g" },
        { name: "Maple syrup", quantity: 60, unit: "ml" },
      ],
    },
    {
      name: "Grilled Cheese Sandwich",
      mealType: "lunch",
      description: "Classic comfort food",
      servings: 2,
      prepTime: 10,
      ingredients: [
        { name: "Bread", quantity: 4, unit: "pcs" },
        { name: "Cheddar cheese", quantity: 100, unit: "g" },
        { name: "Butter", quantity: 20, unit: "g" },
      ],
    },
    {
      name: "Spaghetti Bolognese",
      mealType: "dinner",
      description: "Family favorite pasta",
      servings: 4,
      prepTime: 45,
      ingredients: [
        { name: "Spaghetti", quantity: 400, unit: "g" },
        { name: "Ground beef", quantity: 500, unit: "g" },
        { name: "Canned tomatoes", quantity: 400, unit: "g" },
        { name: "Onion", quantity: 1, unit: "pcs" },
        { name: "Garlic", quantity: 3, unit: "pcs" },
        { name: "Olive oil", quantity: 2, unit: "tbsp" },
        { name: "Parmesan", quantity: 50, unit: "g" },
      ],
    },
    {
      name: "Chicken Stir Fry",
      mealType: "dinner",
      description: "Quick and healthy",
      servings: 4,
      prepTime: 25,
      ingredients: [
        { name: "Chicken breast", quantity: 500, unit: "g" },
        { name: "Broccoli", quantity: 200, unit: "g" },
        { name: "Bell pepper", quantity: 2, unit: "pcs" },
        { name: "Soy sauce", quantity: 3, unit: "tbsp" },
        { name: "Rice", quantity: 300, unit: "g" },
        { name: "Sesame oil", quantity: 1, unit: "tbsp" },
      ],
    },
    {
      name: "Oatmeal with Fruit",
      mealType: "breakfast",
      description: "Healthy start to the day",
      servings: 2,
      prepTime: 10,
      ingredients: [
        { name: "Oats", quantity: 100, unit: "g" },
        { name: "Milk", quantity: 400, unit: "ml" },
        { name: "Banana", quantity: 1, unit: "pcs" },
        { name: "Blueberries", quantity: 100, unit: "g" },
        { name: "Honey", quantity: 1, unit: "tbsp" },
      ],
    },
  ];

  for (const recipe of recipes) {
    await prisma.recipe.create({
      data: {
        name: recipe.name,
        mealType: recipe.mealType,
        description: recipe.description,
        servings: recipe.servings,
        prepTime: recipe.prepTime,
        ingredients: { create: recipe.ingredients },
      },
    });
  }

  console.log(`Seeded ${recipes.length} recipes`);
}

main()
  .catch(console.error)
  .finally(() => prisma.$disconnect());
```

**Step 2: Add seed script to package.json**

Add to `package.json`:
```json
"prisma": {
  "seed": "npx tsx prisma/seed.ts"
}
```

**Step 3: Run seed**

```bash
npm install --save-dev tsx
npx prisma db seed
```

Expected: `Seeded 5 recipes`

**Step 4: Commit**

```bash
git add .
git commit -m "feat: seed 5 sample recipes for initial data"
```

---

## Summary

| Phase | Tasks | What it builds |
|-------|-------|----------------|
| 1 | 1-2 | Project scaffold + DB schema |
| 2 | 3-4 | Recipe list + add recipe form |
| 3 | 5 | 10-day meal planner grid |
| 4 | 6 | Auto-generated grocery list |
| 5 | 7-8 | Navigation + sample data |

**Total:** 8 tasks, ~2-3 hours of focused work.

**After MVP:** Add family preferences, AI suggestions, recipe search, meal variety scoring, and grocery delivery integration.
