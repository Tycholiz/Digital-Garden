
The strategy pattern (a.k.a policy pattern) allows us to write client code that determines which algorithm to select at runtime.
- instead of implementing a single algoritm, we implement multiple, and based on some condition, select which algorithm to run.
- doing this allows the calling code to be more flexible and reusable.

In the following example, we have some client code which needs to update the fields of a food item. The thing each, there are different classes of food items (those that are organic, those that are non-perishable, etc), and they all need to be handled differently. For instance, organic items degrade twice as fast, while non-perishable items don't degrade at all.

```ts
export interface FoodItemStrategy {
  update(item: FoodItem): void;
}

export class PerishableFoodItemStrategy implements FoodItemStrategy {
  update(item: FoodItem): void {
    if (item.doesItemImproveWithAge()) {
      item.increaseQuality();
    } else if (item.getSellInDaysValue() <= 0) {
      item.decreaseQuality(2);
    } else {
      item.decreaseQuality();
    }
    item.decrementSellIn();
  }
}

export class OrganicFoodItemStrategy implements FoodItemStrategy {
  update(item: FoodItem): void {
    if (item.getSellInDaysValue() <= 0) {
      item.decreaseQuality(4);
    } else {
      item.decreaseQuality(2);
    }
    item.decrementSellIn();
  }
}

export class NonPerishableFoodItemStrategy implements FoodItemStrategy {
  update(item: FoodItem): void {
    // no-op
  }
}

/* Implementation */
export class StoreInventory {
  constructor(public items: FoodItem[]) {}

  private updateFoodItem(item: FoodItem): void {
    let strategy: FoodItemStrategy;
    if (item.isItemNonPerishable()) {
      strategy = new NonPerishableFoodItemStrategy();
    } else if (item.isItemOrganic()) {
      strategy = new OrganicFoodItemStrategy();
    } else {
      strategy = new PerishableFoodItemStrategy();
    }
    strategy.update(item);
  }
}
```