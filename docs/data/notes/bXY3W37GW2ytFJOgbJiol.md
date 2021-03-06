
IoC means letting other code call you rather than insisting on doing the calling.
- ex. An example of IoC without dependency injection is the template method pattern. Here, polymorphism is achieved through subclassing, that is, inheritance.

If you follow these simple two steps, you have done inversion of control:
1. Separate what-to-do part from when-to-do part.
2. Ensure that when part knows as little as possible about what part; and vice versa.

## Examples

### Email Example
#### The Scenario
Imagine we had a module called `FortuneCookieEmailer`, which sends email. It uses some kind of service to do the work of actually sending the emails, `BillsBulkEmailService`. Here, the `FortuneCookieEmailer` code now contains a direct reference to the `BillsBulkEmailService`.

This works, but imagine our program expands and now `FortuneCookieEmailer` has to sometimes use `KylesSpammingService`. We would have to go into the `FortuneCookieEmailer` module and introduce logic that would make it conditionally use either `BillsBulkEmailService` or `KylesSpammingService`. Now we need to have some kind of config file somewhere, or some kind of extra "switch" for our `FortuneCookieEmailer` module. The bad thing is that we are adding the job of "deciding which to use" to a module whose sole responsibility should be fortune cookie emails.

#### The solution
Instead of `FortuneCookieEmailer` going out and getting a reference to the `BillsBulkEmailService` or `KylesSpammingService` itself and then using that, `FortuneCookieEmailer` should be provided with a reference to the desired service by something outside of `FortuneCookieEmailer`.

#### Summary
- First situation: FortuneCookieEmailer actively goes out and gets the service that it needs.
- Second (IoC) situation: Someone else gives FortuneCookieEmailer the service that it should use.

The benefit of the second approach is that we can configure our application to use different email services without having to even touch `FortuneCookieEmailer`

### Text Editor Examples
say your application has a text editor component and you want to provide spell checking. Your standard code would look something like this:
```cs
public class TextEditor {

	private SpellChecker checker;

	public TextEditor() {
			this.checker = new SpellChecker();
	}
}
```
What we've done here creates a dependency between the TextEditor and the SpellChecker. In an IoC scenario we would instead do something like this:

```cs
public class TextEditor {

	private IocSpellChecker checker;

	public TextEditor(IocSpellChecker checker) {
			this.checker = checker;
	}
}
```

In the first code example we are instantiating `SpellChecker` (`this.checker = new SpellChecker();`), which means the `TextEditor` class directly depends on the `SpellChecker` class.

In the second code example we are creating an abstraction by having the `SpellChecker` dependency class in `TextEditor`'s constructor signature (not initializing dependency in class). This allows us to call the dependency then pass it to the `TextEditor` class like so:

```cs
SpellChecker sc = new SpellChecker(); // dependency
TextEditor textEditor = new TextEditor(sc);
```

Now the client creating the TextEditor class has control over which SpellChecker implementation to use because we're injecting the dependency into the TextEditor signature.

### Analogy to understand IoC
Inversion of Control, (or IoC), is about getting freedom (You get married, you lost freedom and you are being controlled. You divorced, you have just implemented Inversion of Control. That's what we called, "decoupled". Good computer system discourages some very close relationship.) more flexibility (The kitchen in your office only serves clean tap water, that is your only choice when you want to drink. Your boss implemented Inversion of Control by setting up a new coffee machine. Now you get the flexibility of choosing either tap water or coffee.) and less dependency (Your partner has a job, you don't have a job, you financially depend on your partner, so you are controlled. You find a job, you have implemented Inversion of Control. Good computer system encourages in-dependency.)

With the above ideas in mind. We still miss a key part of IoC. In the scenario of IoC, the software/object consumer is a sophisticated framework. That means the code you created is not called by yourself. Now let's explain why this way works better for a web application.

A traditional software framework will be like a garage with many tools. So the workers need to make a plan themselves and use the tools to build the car. Building a car is not an easy business, it will be really hard for the workers to plan and cooperate properly. A modern software framework will be like a modern car factory with all the facilities and managers in place. The workers do not have to make any plan, the managers (part of the framework, they are the smartest people and made the most sophisticated plan) will help coordinate so that the workers know when to do their job (framework calls your code). The workers just need to be flexible enough to use any tools the managers give to them (by using Dependency Injection).

Although the workers give the control of managing the project on the top level to the managers (the framework). But it is good to have some professionals help out. This is the concept of IoC truly come from.

Dependency Injection and Inversion of Control are related. Dependency Injection is at the micro level and Inversion of Control is at the macro level. You have to eat every bite (implement DI) in order to finish a meal (implement IoC).


# UE Resources
[Martin Fowler on IoC](https://martinfowler.com/bliki/InversionOfControl.html)
