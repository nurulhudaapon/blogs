---
title: "Programming Design Patterns"
datePublished: Sat Jul 13 2024 11:27:57 GMT+0000 (Coordinated Universal Time)
cuid: clyk1m28z00070ajt0im4eju7
slug: 30-programming-design-patterns
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1720871678776/95a92760-1947-48b7-b6f9-7c767960909f.png

---
### **Singleton Pattern**

Ensures a class has only one instance and provides a global point of access to it.

**When to use**: When exactly one instance of a class is needed to control the action.  
**Why to use**: To prevent multiple instances of a class and provide a single point of access.

```typescript
class Singleton {
    private static instance: Singleton;
    private constructor() {}
    static getInstance(): Singleton {
        if (!Singleton.instance) {
            Singleton.instance = new Singleton();
        }
        return Singleton.instance;
    }
}
const instance1 = Singleton.getInstance();
const instance2 = Singleton.getInstance();
console.log(instance1 === instance2); // true
```

### **Factory Pattern**

Creates objects without specifying the exact class of object that will be created.

**When to use**: When the exact type of the object is determined at runtime.  
**Why to use**: To encapsulate object creation.

```typescript
interface Product {
    operation(): string;
}

class ConcreteProduct1 implements Product {
    operation(): string {
        return 'ConcreteProduct1';
    }
}

class ConcreteProduct2 implements Product {
    operation(): string {
        return 'ConcreteProduct2';
    }
}

class Creator {
    static factoryMethod(type: string): Product {
        if (type === '1') {
            return new ConcreteProduct1();
        } else {
            return new ConcreteProduct2();
        }
    }
}

const product1 = Creator.factoryMethod('1');
console.log(product1.operation()); // ConcreteProduct1
```

### **Abstract Factory Pattern**

Provides an interface for creating families of related or dependent objects without specifying their concrete classes.

**When to use**: When families of related objects need to be created.  
**Why to use**: To ensure consistency among products.

```typescript
interface AbstractFactory {
    createProductA(): AbstractProductA;
    createProductB(): AbstractProductB;
}

class ConcreteFactory1 implements AbstractFactory {
    createProductA(): AbstractProductA {
        return new ProductA1();
    }
    createProductB(): AbstractProductB {
        return new ProductB1();
    }
}

interface AbstractProductA {
    usefulFunctionA(): string;
}

interface AbstractProductB {
    usefulFunctionB(): string;
}

class ProductA1 implements AbstractProductA {
    usefulFunctionA(): string {
        return 'The result of ProductA1';
    }
}

class ProductB1 implements AbstractProductB {
    usefulFunctionB(): string {
        return 'The result of ProductB1';
    }
}

const factory1 = new ConcreteFactory1();
const productA1 = factory1.createProductA();
const productB1 = factory1.createProductB();
console.log(productA1.usefulFunctionA()); // The result of ProductA1
console.log(productB1.usefulFunctionB()); // The result of ProductB1
```

### **Builder Pattern**

Separates the construction of a complex object from its representation, allowing the same construction process to create various representations.

**When to use**: When an object needs to be created with various configurations.  
**Why to use**: To construct a complex object step by step.

```typescript
class Product {
    public parts: string[] = [];
    public listParts(): void {
        console.log(`Product parts: ${this.parts.join(', ')}`);
    }
}

class Builder {
    private product: Product;
    constructor() {
        this.product = new Product();
    }
    public reset(): void {
        this.product = new Product();
    }
    public addPartA(): void {
        this.product.parts.push('PartA');
    }
    public addPartB(): void {
        this.product.parts.push('PartB');
    }
    public getProduct(): Product {
        return this.product;
    }
}

const builder = new Builder();
builder.addPartA();
builder.addPartB();
const product = builder.getProduct();
product.listParts(); // Product parts: PartA, PartB
```

### **Prototype Pattern**

Creates new objects by copying an existing object, known as the prototype.

**When to use**: When the cost of creating a new object is expensive.  
**Why to use**: To reduce the cost of creating objects.

```typescript
class Prototype {
    public primitive: any;
    public component: object;
    public circularReference: ComponentWithBackReference;

    public clone(): this {
        const clone = Object.create(this);
        clone.component = Object.create(this.component);
        clone.circularReference = {
            ...this.circularReference,
            prototype: { ...this }
        };
        return clone;
    }
}

class ComponentWithBackReference {
    public prototype;
    constructor(prototype: Prototype) {
        this.prototype = prototype;
    }
}

const prototype = new Prototype();
prototype.primitive = 245;
prototype.component = new Date();
prototype.circularReference = new ComponentWithBackReference(prototype);

const clone = prototype.clone();
console.log(clone.primitive); // 245
console.log(clone.component); // Date object
console.log(clone.circularReference.prototype === prototype); // false
```

### **Adapter Pattern**

Allows incompatible interfaces to work together.

**When to use**: When you want to use an existing class, but its interface is not compatible with the rest of your code.  
**Why to use**: To enable collaboration between classes with incompatible interfaces.

```typescript
class Target {
    request(): string {
        return 'Target: The default target\'s behavior.';
    }
}

class Adaptee {
    specificRequest(): string {
        return '.eetpadA eht fo roivaheb laicepS';
    }
}

class Adapter extends Target {
    private adaptee: Adaptee;

    constructor(adaptee: Adaptee) {
        super();
        this.adaptee = adaptee;
    }

    request(): string {
        return `Adapter: (TRANSLATED) ${this.adaptee.specificRequest().split('').reverse().join('')}`;
    }
}

const adaptee = new Adaptee();
const adapter = new Adapter(adaptee);
console.log(adapter.request()); // Adapter: (TRANSLATED) Special behavior of the Adaptee.
```

### **Composite Pattern**

Composes objects into tree structures to represent part-whole hierarchies.

**When to use**: When you need to treat individual objects and compositions of objects uniformly.  
**Why to use**: To simplify client code that deals with complex tree structures.

```typescript
interface Component {
    operation(): string;
}

class Leaf implements Component {
    operation(): string {
        return 'Leaf';
    }
}

class Composite implements Component {
    private children: Component[] = [];
    
    add(component: Component): void {
        this.children.push(component);
    }

    remove(component: Component): void {
        const index = this.children.indexOf(component);
        this.children.splice(index, 1);
    }

    operation(): string {
        const results = [];
        for (const child of this.children) {
            results.push(child.operation());
        }
        return `Branch(${results.join('+')})`;
    }
}

const leaf = new Leaf();
const tree = new Composite();
const branch1 = new Composite();
const branch2 = new Composite();

branch1.add(leaf);
branch2.add(leaf);
tree.add(branch1);
tree.add(branch2);

console.log(tree.operation()); // Branch(Branch(Leaf)+Branch(Leaf))
```

### **Decorator Pattern**

Adds additional responsibilities to an object dynamically.

**When to use**: When you need to add responsibilities to objects dynamically.  
**Why to use**: To avoid subclassing for functionalities.

```typescript
interface Component {
    operation(): string;
}

class ConcreteComponent implements Component {
    operation(): string {
        return 'ConcreteComponent';
    }
}

class Decorator implements Component {
    protected component: Component;

    constructor(component: Component) {
        this.component = component;
    }

    operation(): string {
        return this.component.operation();
    }
}

class ConcreteDecoratorA extends Decorator {
    operation(): string {
        return `ConcreteDecoratorA(${super.operation()})`;
    }
}

class ConcreteDecoratorB extends Decorator {
    operation(): string {
        return `ConcreteDecoratorB(${super.operation()})`;
    }


}

const simple = new ConcreteComponent();
console.log(simple.operation()); // ConcreteComponent

const decorator1 = new ConcreteDecoratorA(simple);
const decorator2 = new ConcreteDecoratorB(decorator1);
console.log(decorator2.operation()); // ConcreteDecoratorB(ConcreteDecoratorA(ConcreteComponent))
```

### **Facade Pattern**

Provides a simplified interface to a complex subsystem.

**When to use**: When you need to provide a simple interface to a complex subsystem.  
**Why to use**: To hide the complexities of the system.

```typescript
class Subsystem1 {
    operation1(): string {
        return 'Subsystem1: Ready!';
    }
    operationN(): string {
        return 'Subsystem1: Go!';
    }
}

class Subsystem2 {
    operation1(): string {
        return 'Subsystem2: Get ready!';
    }
    operationZ(): string {
        return 'Subsystem2: Fire!';
    }
}

class Facade {
    protected subsystem1: Subsystem1;
    protected subsystem2: Subsystem2;

    constructor(subsystem1: Subsystem1, subsystem2: Subsystem2) {
        this.subsystem1 = subsystem1;
        this.subsystem2 = subsystem2;
    }

    operation(): string {
        let result = 'Facade initializes subsystems:\n';
        result += this.subsystem1.operation1() + '\n';
        result += this.subsystem2.operation1() + '\n';
        result += 'Facade orders subsystems to perform the action:\n';
        result += this.subsystem1.operationN() + '\n';
        result += this.subsystem2.operationZ();
        return result;
    }
}

const subsystem1 = new Subsystem1();
const subsystem2 = new Subsystem2();
const facade = new Facade(subsystem1, subsystem2);
console.log(facade.operation());
```

### **Flyweight Pattern**

Uses sharing to support large numbers of fine-grained objects efficiently.

**When to use**: When a large number of similar objects are needed.  
**Why to use**: To reduce memory consumption.

```typescript
class Flyweight {
    private sharedState: any;

    constructor(sharedState: any) {
        this.sharedState = sharedState;
    }

    operation(uniqueState: any): void {
        const s = JSON.stringify(this.sharedState);
        const u = JSON.stringify(uniqueState);
        console.log(`Flyweight: Displaying shared (${s}) and unique (${u}) state.`);
    }
}

class FlyweightFactory {
    private flyweights: { [key: string]: Flyweight } = {};

    constructor(initialFlyweights: string[][]) {
        for (const state of initialFlyweights) {
            this.flyweights[this.getKey(state)] = new Flyweight(state);
        }
    }

    private getKey(state: string[]): string {
        return state.join('_');
    }

    getFlyweight(sharedState: string[]): Flyweight {
        const key = this.getKey(sharedState);

        if (!(key in this.flyweights)) {
            console.log('FlyweightFactory: Can\'t find a flyweight, creating new one.');
            this.flyweights[key] = new Flyweight(sharedState);
        } else {
            console.log('FlyweightFactory: Reusing existing flyweight.');
        }

        return this.flyweights[key];
    }
}

const factory = new FlyweightFactory([
    ['Chevrolet', 'Camaro2018', 'pink'],
    ['Mercedes Benz', 'C300', 'black'],
    ['Mercedes Benz', 'C500', 'red'],
]);

const flyweight1 = factory.getFlyweight(['Chevrolet', 'Camaro2018', 'pink']);
flyweight1.operation(['John Doe', 'CL234IR']);
const flyweight2 = factory.getFlyweight(['Mercedes Benz', 'C300', 'black']);
flyweight2.operation(['Jane Doe', 'CL234IR']);
```

### **Proxy Pattern**

Provides a surrogate or placeholder for another object to control access to it.

**When to use**: When you need a more versatile or sophisticated reference to an object.  
**Why to use**: To control access and add additional functionality.

```typescript
interface Subject {
    request(): void;
}

class RealSubject implements Subject {
    request(): void {
        console.log('RealSubject: Handling request.');
    }
}

class Proxy implements Subject {
    private realSubject: RealSubject;

    constructor(realSubject: RealSubject) {
        this.realSubject = realSubject;
    }

    request(): void {
        if (this.checkAccess()) {
            this.realSubject.request();
            this.logAccess();
        }
    }

    private checkAccess(): boolean {
        console.log('Proxy: Checking access prior to firing a real request.');
        return true;
    }

    private logAccess(): void {
        console.log('Proxy: Logging the time of request.');
    }
}

const realSubject = new RealSubject();
const proxy = new Proxy(realSubject);
proxy.request(); // Proxy: Checking access prior to firing a real request. RealSubject: Handling request. Proxy: Logging the time of request.
```

### **Command Pattern**

Turns a request into a stand-alone object that contains all information about the request.

**When to use**: When you want to parameterize objects with operations.  
**Why to use**: To encapsulate requests as objects.

```typescript
interface Command {
    execute(): void;
}

class SimpleCommand implements Command {
    private payload: string;

    constructor(payload: string) {
        this.payload = payload;
    }

    public execute(): void {
        console.log(`SimpleCommand: See, I can do simple things like printing (${this.payload})`);
    }
}

class ComplexCommand implements Command {
    private receiver: Receiver;
    private a: string;
    private b: string;

    constructor(receiver: Receiver, a: string, b: string) {
        this.receiver = receiver;
        this.a = a;
        this.b = b;
    }

    public execute(): void {
        console.log('ComplexCommand: Complex stuff should be done by a receiver object.');
        this.receiver.doSomething(this.a);
        this.receiver.doSomethingElse(this.b);
    }
}

class Receiver {
    public doSomething(a: string): void {
        console.log(`Receiver: Working on (${a}).`);
    }

    public doSomethingElse(b: string): void {
        console.log(`Receiver: Also working on (${b}).`);
    }
}

class Invoker {
    private onStart: Command;
    private onFinish: Command;

    public setOnStart(command: Command): void {
        this.onStart = command;
    }

    public setOnFinish(command: Command): void {
        this.onFinish = command;
    }

    public doSomethingImportant(): void {
        if (this.onStart) {
            this.onStart.execute();
        }

        if (this.onFinish) {
            this.onFinish.execute();
        }
    }
}

const invoker = new Invoker();
invoker.setOnStart(new SimpleCommand('Say Hi!'));
const receiver = new Receiver();
invoker.setOnFinish(new ComplexCommand(receiver, 'Send email', 'Save report'));

invoker.doSomethingImportant();
```

### **Iterator Pattern**

Provides a way to access elements of a collection sequentially without exposing its underlying representation.

**When to use**: When you need to traverse a collection without exposing its implementation.  
**Why to use**: To provide a standard way to iterate over a collection.

```typescript
interface Iterator<T> {
    next(): T;
    hasNext(): boolean;
}

interface Aggregator {
    getIterator(): Iterator<string>;
}

class ConcreteIterator implements Iterator<string> {
    private collection: ConcreteCollection;
    private position: number = 0;

    constructor(collection: ConcreteCollection) {
        this.collection = collection;
    }

    public next(): string {
        const item = this.collection.items[this.position];
        this.position += 1;
        return item;
    }

    public hasNext(): boolean {
        return this.position < this.collection.items.length;
    }
}

class ConcreteCollection implements Aggregator {
    public items: string[] = [];

    public getIterator(): Iterator<string> {
        return new ConcreteIterator(this);
    }
}

const collection = new ConcreteCollection();
collection.items.push('Item 1');
collection.items.push('Item 2');
collection.items.push('Item 3');

const iterator = collection.get

Iterator();
while (iterator.hasNext()) {
    console.log(iterator.next());
}
```

### **Mediator Pattern**

Defines an object that encapsulates how a set of objects interact.

**When to use**: When you need to reduce the complexity of communication between objects.  
**Why to use**: To promote loose coupling by keeping objects from referring to each other explicitly.

```typescript
interface Mediator {
    notify(sender: object, event: string): void;
}

class ConcreteMediator implements Mediator {
    private component1: Component1;
    private component2: Component2;

    constructor(c1: Component1, c2: Component2) {
        this.component1 = c1;
        this.component1.setMediator(this);
        this.component2 = c2;
        this.component2.setMediator(this);
    }

    public notify(sender: object, event: string): void {
        if (event === 'A') {
            console.log('Mediator reacts on A and triggers following operations:');
            this.component2.doC();
        }

        if (event === 'D') {
            console.log('Mediator reacts on D and triggers following operations:');
            this.component1.doB();
            this.component2.doC();
        }
    }
}

class BaseComponent {
    protected mediator: Mediator;

    public setMediator(mediator: Mediator): void {
        this.mediator = mediator;
    }
}

class Component1 extends BaseComponent {
    public doA(): void {
        console.log('Component 1 does A.');
        this.mediator.notify(this, 'A');
    }

    public doB(): void {
        console.log('Component 1 does B.');
        this.mediator.notify(this, 'B');
    }
}

class Component2 extends BaseComponent {
    public doC(): void {
        console.log('Component 2 does C.');
        this.mediator.notify(this, 'C');
    }

    public doD(): void {
        console.log('Component 2 does D.');
        this.mediator.notify(this, 'D');
    }
}

const c1 = new Component1();
const c2 = new Component2();
const mediator = new ConcreteMediator(c1, c2);

c1.doA();
c2.doD();
```

### **Memento Pattern**

Captures and externalizes an object's internal state without violating encapsulation, so the object can be restored to this state later.

**When to use**: When you need to restore an object to a previous state.  
**Why to use**: To provide the ability to restore an object to its previous state.

```typescript
class Memento {
    private state: string;

    constructor(state: string) {
        this.state = state;
    }

    public getState(): string {
        return this.state;
    }
}

class Originator {
    private state: string;

    public setState(state: string): void {
        console.log(`Originator: Setting state to ${state}`);
        this.state = state;
    }

    public save(): Memento {
        return new Memento(this.state);
    }

    public restore(memento: Memento): void {
        this.state = memento.getState();
        console.log(`Originator: State after restoring from Memento: ${this.state}`);
    }
}

class Caretaker {
    private mementos: Memento[] = [];
    private originator: Originator;

    constructor(originator: Originator) {
        this.originator = originator;
    }

    public backup(): void {
        console.log('Caretaker: Saving Originator\'s state...');
        this.mementos.push(this.originator.save());
    }

    public undo(): void {
        if (!this.mementos.length) {
            return;
        }
        const memento = this.mementos.pop();
        console.log('Caretaker: Restoring state to:', memento.getState());
        this.originator.restore(memento);
    }
}

const originator = new Originator();
const caretaker = new Caretaker(originator);

originator.setState('State1');
caretaker.backup();
originator.setState('State2');
caretaker.backup();
originator.setState('State3');

caretaker.undo(); // Caretaker: Restoring state to: State2
caretaker.undo(); // Caretaker: Restoring state to: State1
```

### **Observer Pattern**

Defines a one-to-many dependency between objects so that when one object changes state, all its dependents are notified and updated automatically.

**When to use**: When you need many other objects to receive an update when another object changes.  
**Why to use**: To allow a single object to notify multiple other objects about changes in its state.

```typescript
interface Observer {
    update(subject: Subject): void;
}

class ConcreteObserverA implements Observer {
    public update(subject: Subject): void {
        if (subject.state < 3) {
            console.log('ConcreteObserverA: Reacted to the event.');
        }
    }
}

class ConcreteObserverB implements Observer {
    public update(subject: Subject): void {
        if (subject.state === 0 || subject.state >= 2) {
            console.log('ConcreteObserverB: Reacted to the event.');
        }
    }
}

class Subject {
    public state: number;
    private observers: Observer[] = [];

    public attach(observer: Observer): void {
        const isExist = this.observers.includes(observer);
        if (isExist) {
            return console.log('Subject: Observer has been attached already.');
        }
        this.observers.push(observer);
    }

    public detach(observer: Observer): void {
        const observerIndex = this.observers.indexOf(observer);
        if (observerIndex === -1) {
            return console.log('Subject: Nonexistent observer.');
        }
        this.observers.splice(observerIndex, 1);
    }

    public notify(): void {
        console.log('Subject: Notifying observers...');
        for (const observer of this.observers) {
            observer.update(this);
        }
    }

    public someBusinessLogic(): void {
        console.log('Subject: I\'m doing something important.');
        this.state = Math.floor(Math.random() * 10);
        console.log(`Subject: My state has just changed to: ${this.state}`);
        this.notify();
    }
}

const subject = new Subject();

const observer1 = new ConcreteObserverA();
subject.attach(observer1);

const observer2 = new ConcreteObserverB();
subject.attach(observer2);

subject.someBusinessLogic();
subject.someBusinessLogic();
```