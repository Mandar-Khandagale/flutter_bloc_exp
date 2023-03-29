# BLOC :—
-- Business Logic Component 

— Used to separate UI with business logic and for proper state management

### BLOC Core Concepts :—

1. Stream :—
	—  Continuous flow of data 
	—  async* (async generator) - to send async data
	— yield - needs to push data continuously in stream
	— await - helps to awaiting a process

Eg :—
 ```
Stream<int> boatStream() async*{
  for(var i=1; i<=10; i++){
    print("Sended the data Stream no:- $i");
    await Future.delayed(Duration(milliseconds: 1000));
    yield i;
  }
}
```
````
void main() {  
  Stream<int> boat = boatStream();

  boat.listen((data){
    print("Recieved Data stream no:- $data");
  });
  
}
````

————————————————————————————————————————————————————————————————

## BLOC vs Cubit 
— Cubit is minimal version of bloc (Bloc extends Cubit)

— What is Cubit?
	— Cubit is a special kind of Stream Component, which is based on functions called from UI which will rebuilt the UI after emitting different states on stream
	— compare to Bloc, Cubit (functions)  are not part of a Stream but it has a predefined  stand alone list of , what can be done inside a cubit. It has only one stream I.e stream if states

— What is Bloc?
	— Bloc has both stream, event of stream and states of stream

1. When should we create a Cubit or Bloc in project ?
	— if screen has many things to do then we should use Bloc or else if there are minor changes on the screen then we should use Cubit

2. When should I use cubit or bloc 
	— when there is a small event like increment or decrement count then use cubit, otherwise use bloc

————————————————————————————————————————————————————————————————

## Flutter Bloc  Concept :—

### BlocProvider :—

<b>BlocProvider</b> is a widget, which create and builds a bloc for its children widget. It is also known as DI as its just provide a single instance of Bloc to all its children widget

To access a Bloc in subtree use BlocProvider.of<BlocA>(context); or context.bloc<BlocA>();

By default  Bloc will create a lazy instance, so that it will only created when  its instance called and it will also dispose when its not in use

BlocProvider.value() is used when we initialised our Bloc in one class and need that same instance in other class,
In these case BlocProvider won’t close the Bloc automatically because it may be required in the previous class


### BlocBuilder :—

This is the easy one. This is used when we want to draw a Widget based on what is the current State. In the following example a new “text” gets drawn every time the state changes.

buildWhen (Optional):—
 This is flag (true/false) indicates if the builder method should be called or not, keep in mind that this is called only during a rebuild process (explained at the bottom of the article). If this returns true then builder is called, if returns false it is not called. If buildWhen is not declared then the builder is always executed.

builder (Required):— This method is most important, this returns the widget that we want to draw based on the current state. i.e. The state is “OrderCompleted” then it returns “Text(‘Order Completed!’)”


### BlocListener :—

This is just a listener not a builder (like the above), that means that its job is keep listening for new changes in the state and not to return a widget. Each time the state changes to a new state this listener will receive a notification that the state has changed and then you can trigger an action (e.g. Send a notification, consume an endpoint, analytics, etc).
So what gets draw in the screen doesn’t depends of what we receive in the listener, i.e. it doesn’t depend of the actual state (OrdersState), it reacts depending of the state.

listenWhen (Optional):—
 This is flag (true/false) indicates if the listener method should be called or not, keep in mind that this is called only during a rebuild process (explained at the bottom of the article). If this returns true then listener is called, if returns false it is not called. If listenWhen is not declared then the listener is always 

listener:—This method is most important, it listens for new changes in the state and execute actions based on the received state. For example: API requests, call analytics stuff, etc.

### BlocConsumer :—

This is a mix between “BlocListener” and “BlocBuilder”. This is used when we want to draw something based on the current state and execute some actions depending on the new arriving states.


NOTE :—
“listenWhen” and “buildWhen” are called only during a rebuild process. i.e these methods are not called the first time a widget is drawn, it is called only during redraw; so the widget is drawn using the current state, then later a rebuild process is executed and these methods are called “listenWhen” and “buildWhen” in order to know if it should proceed to draw again the widget (build) or call an action (in listen).
