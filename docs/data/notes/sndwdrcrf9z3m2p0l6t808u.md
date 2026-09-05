
```ts
class Semaphore {
  constructor(maxConcurrency) {
    this.maxConcurrency = maxConcurrency;
    this.currentConcurrency = 0;
    this.queue = [];
  }

  async acquire() {
    if (this.currentConcurrency < this.maxConcurrency) {
      this.currentConcurrency++;
      return;
    }

    return new Promise((resolve) => this.queue.push(resolve));
  }

  release() {
    if (this.queue.length > 0) {
      const nextResolve = this.queue.shift();
      nextResolve();
    } else {
      this.currentConcurrency--;
    }
  }
}

async function controlledAsyncAction(semaphore, index) {
  await semaphore.acquire();
  try {
    await asyncAction(index);
  } finally {
    semaphore.release();
  }
}

async function performControlledAsyncActions() {
  const semaphore = new Semaphore(3); // Limit to 3 concurrent actions
  const promises = [];
  for (let i = 0; i < 10; i++) {
    promises.push(controlledAsyncAction(semaphore, i));
  }
  await Promise.all(promises);
  console.log('All controlled async actions completed');
}

performControlledAsyncActions();
```