# Basic structure
React apps are made out of components. A component is a piece of the UI (user interface) that has its own logic and appearance. A component can be as small as a button, or as large as an entire page.

React components are JavaScript functions that return markup:
```
function MyButton() {
  return (
    <button>I'm a button</button>
  );
}
```
Now that you’ve declared MyButton, you can nest it into another component:
```
export default function MyApp() {
  return (
    <div>
      <h1>Welcome to my app</h1>
      <MyButton />
    </div>
  );
}
```

# notes:
The export default keywords specify the main component in the file

to Display the Javascript function which express react component , use below way:  <MyButton/> ， 注意： 没有括号

JavaScript functions that return markup as react compones follow normal Markdown syntex. See MDN

# markup with JSX
JSX is stricter than HTML

You have to close tags like
```
<br />
```
Your component also can’t return multiple JSX tags

You have to wrap them into a shared parent, like a <div>...</div> or an empty <>...</> wrapper:
```
function AboutPage() {
  return (
    <>
      <h1>About</h1>
      <p>Hello there.<br />How do you do?</p>
    </>
  );
}
```
# Passing props to a component 
 React components use props to communicate with each other. Every parent component can pass some information to its child components by giving them props. Props might 
 remind you of HTML attributes, but you can pass any JavaScript value through them, including objects, arrays, functions, and even JSX!

 all props are included into {} 
 
 function Card({ children }) {}
 
 function Avatar({ person, size }){}
```
import { getImageUrl } from './utils.js'

export default function Profile() {
  return (
    <Card>
      <Avatar
        size={100}
        person={{
          name: 'Katsuko Saruhashi',
          imageId: 'YfeOqp2'
        }}
      />
    </Card>
  );
}

function Avatar({ person, size }) {
  return (
    <img
      className="avatar"
      src={getImageUrl(person)}
      alt={person.name}
      width={size}
      height={size}
    />
  );
}

function Card({ children }) {
  return (
    <div className="card">
      {children}
    </div>
  );
}
```
 
 
# Adding styles
   like normal html
# Displaying data 
  用{}括起来把数据(变量， 常量，img src） 显示在Markdown 元素的内容和属性中
  
  JSX lets you put markup into JavaScript. Curly braces let you “escape back” into JavaScript so that you can embed some variable from your code and display it to the user. For example, this will display user.name:
  
  You can also “escape into JavaScript” from JSX attributes, but you have to use curly braces instead of quotes. For example, className="avatar" passes the "avatar" string as the CSS class, but src={user.imageUrl} reads the JavaScript user.imageUrl variable value, and then passes that value as the src attribute:
  ```
  return (
  <img
    className="avatar"
    src={user.imageUrl}
  />
);
  ```
  seems {} 可以把任何要显示在JSX 中的元素包在里面, 以及嵌套， 类似加减乘除运算式中的小括号嵌套
  ```
  import { useState } from 'react';

export default function MyApp() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1);
  }

  return (
    <div>
      <h1>Counters that update together</h1>
      <MyButton count={count} onClick={handleClick} />
      <MyButton count={count} onClick={handleClick} />
    </div>
  );
}

function MyButton({ count, onClick }) {
  return (
    <button onClick={onClick}>
      Clicked {count} times
    </button>
  );
}

  ```
# Conditional rendering
# Rendering lists 
 You will rely on JavaScript features like for loop and the array map() function to render lists of components
 # Responding to events
 You can respond to events by declaring event handler functions inside your components:
 #  Add state to your component to remeber something
 ```
function MyButton() {
  const [count, setCount] = useState(0);
  // ...

You’ll get two things from useState: the current state (count), and the function that lets you update it (setCount). You can give them any names, but the convention is to write [something, setSomething].
```
# Using Hooks
Functions starting with use are called Hooks

Hooks are more restrictive than other functions. You can only call Hooks at the top of your components (or other Hooks). If you want to use useState in a condition or a loop, extract a new component and put it there.
