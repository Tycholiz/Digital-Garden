
A proxy, in its most general form, is a class functioning as an interface to something else (the *subject*).
- That interface could be anything, including:
  - a network connection
  - a large object in memory
  - a file
  - some other resource that is expensive or impossible to duplicate

When you see "proxy" think: *"something that stands-in for something else."*

The proxy implements the same interface as the real serving object. Therefore, from the client's perspective, using the proxy and using the real object (ie. the *subject*) itself should be the same.

a proxy is a wrapper or agent object that is being called by the client to access the real serving object (ie. the *subject*) behind the scenes
- the proxy may simply forward the request, or it may provide some additional logic.
  - for example, it may cache data from the serving object, or it may perform some type of check to ensure that the the operation should be permitted (e.g. maybe some business logic must be satisfied before the serving object can be accessed in the first place)

![](/assets/images/2022-04-08-09-04-59.png)

Proxies are about providing an extra level of [[indirection|general.terms.indirection]] to support distributed, controlled, or intelligent access.

```ts
class Driver {
  constructor (age) {
    this.age = age
  }
}

class Car {
  drive () {
    console.log('Car has been driven!')
  }
}

// this proxy checks to make sure the driver is old enough, then calls the serving object (the one instantiated from Car)
class ProxyCar {
  constructor (driver) {
    this.car = new Car()
    this.driver = driver
  }

  drive () {
    if (this.driver.age <= 16) {
      console.log('Sorry, the driver is too young to drive.')
    } else {
      this.car.drive()
    }
  }
}

// Run program
const driver = new Driver(16)
const car = new ProxyCar(driver)
car.drive()

const driver2 = new Driver(25)
const car2 = new ProxyCar(driver2)
car2.drive()
```