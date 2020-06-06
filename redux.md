# Redux

## What is Redux ?

- Redux is state management library for  java script  such as react, angular, veu.
    
## When to use ?

- In a application that have complex UI, we need to listen to change in one part of UI and immediately need to change in another part of UI.

- The change in data can be because of network request  or background task or some asynchronous  task.

- Managing data  or state in such app is very complex with out state management solution. Some of  such state management solution are , 
    - Flux (created by Facebook)
    - Redux (inspired by Flux)
    - Mobx

## How redux works ?
   
- In redux instead of scattering the application state in various part of application UI, we store it in a central repository that is a single javascript object called store (a kind of database for front end)

- So different part of ui no longer need to maintain it own state instead they get or store in store.

- If something goes wrong, Redux can show how the change happened and why transparently.

- In short, Redux centralizes our application state and makes data flow transparent and predictable.


