---
id: reactnative21
title: Code standart
sidebar_label: Code standart
---



## 1. Optional chaining 

```jsx
const human = {
  child: {
    name: '',
    action: () => {...}
  },
  children: [...]
}


// bad
human.child && human.child.name
human.child && human.child.action()
human.child && human.child.children[0]

// good
human.child?.name
human.child?.action?.()
human.child?.children?.[0]
```

## 2. Null-ish coalescing ??

```jsx
undefined ?? 'something' // 'something'
null ?? 'something' // 'something'

0 ?? 'something' // 0
'' ?? 'something' // ''
false ?? 'something' // false
NaN ?? 'something' // NaN

// Example:
const someStringFromTheServer = ''

// bad
someStringFromTheServer || 'something'

// good
someStringFromTheServer ?? 'something'
```

## 3. If the property/method is a boolean, use isVal, hasVal or withVal.

```jsx
// bad
if (!valid) {
  return false;
}

// good
if (!isValid) {
  return false;
}
```

## 4. For type casting use (eslint: no-new-wrappers):

Numbers:
Why? The parseInt function produces an integer value dictated by interpretation of the contents of the string argument according to the specified radix. Leading whitespace in string is ignored. If radix is undefined or 0, it is assumed to be 10 except when the number begins with the character pairs 0x or 0X, in which case a radix of 16 is assumed. This differs from ECMAScript 3, which merely discouraged (but allowed) octal interpretation. Many implementations have not adopted this behavior as of 2013. And, because older browsers must be supported, always specify a radix.

```jsx
const inputValue = '4';

// bad
const val = new Number(inputValue);

// bad
const val = +inputValue;

// bad
const val = inputValue >> 0;

// bad
const val = parseInt(inputValue);

// good
const val = Number(inputValue);

// good
const val = parseInt(inputValue, 10);
Strings:


const reviewScore = 9;

// bad
const totalScore = new String(reviewScore); // typeof totalScore is "object" not "string"

// bad
const totalScore = reviewScore + ''; // invokes this.reviewScore.valueOf()

// bad
const totalScore = reviewScore.toString(); // isn’t guaranteed to return a string

// good
const totalScore = String(reviewScore);
Booleans:


const age = 0;

// bad
const hasAge = new Boolean(age);

// good
const hasAge = Boolean(age);

// best
const hasAge = !!age;
```

## 5. Use the literal syntax for array creation. eslint: no-array-constructor


```jsx
// bad
const items = new Array();

// good
const items = [];
```

## 6. Use Array.from instead of spread ... 
For mapping over iterables, because it avoids creating an intermediate array.

```jsx
// bad
const baz = [...foo].map(bar);

// good
const baz = Array.from(foo, bar);
```

7. Never use arguments, opt to use rest syntax ... instead. eslint: prefer-rest-params

```jsx
// bad
function concatenateAll() {
  const args = Array.prototype.slice.call(arguments);
  return args.join('');
}

// good
function concatenateAll(...args) {
  return args.join('');
}
```

## 8. Always put default parameters last. eslint: default-param-last

```jsx
// bad
function handleThings(opts = {}, name) {
  // ...
}

// good
function handleThings(name, opts = {}) {
  // ...
}
```

## 9. Use exponentiation operator ** 
When calculating exponentiations. eslint: no-restricted-properties

```jsx
// bad
const binary = Math.pow(2, 10);

// good
const binary = 2 ** 10;
```

## 10. Assign variables where you need them, but place them in a reasonable place.

Why? let and const are block-scoped and not function-scoped.

```jsx
// bad - unnecessary function call
function checkName(hasName) {
  const name = getName();

  if (hasName === 'test') {
    return false;
  }

  if (name === 'test') {
    this.setName('');
    return false;
  }

  return name;
}

// good
function checkName(hasName) {
  if (hasName === 'test') {
    return false;
  }

  const name = getName();

  if (name === 'test') {
    this.setName('');
    return false;
  }

  return name;
}
```

## 11. Group all your consts and then group all your lets.

```jsx
// bad
let i, dragonball,
    items = getItems(),
    goSportsTeam = true;

// bad
let i;
const items = getItems();
let dragonball;
const goSportsTeam = true;

// good
const goSportsTeam = true;
const items = getItems();
let dragonball;
let i;
```

## 12. Use shortcuts for booleans, but explicit comparisons for strings and numbers.

```jsx
// bad
if (isValid === true) {
  // ...
}

// good
if (isValid) {
  // ...
}

// bad
if (name) {
  // ...
}

// good
if (name !== '') {
  // ...
}

// bad
if (collection.length) {
  // ...
}

// good
if (collection.length > 0) {
  // ...
}
```

## 13. Avoid unneeded ternary statements. eslint: no-unneeded-ternary

```jsx
// bad
const foo = a ? a : b;
const bar = c ? true : false;
const baz = c ? false : true;

// good
const foo = a || b;
const bar = !!c;
const baz = !c;
```

## 14. Avoid single-letter names. Be descriptive with your naming. eslint: id-length

```jsx
// bad
function q() {
  // ...
}

// good
function query() {
  // ...
}
```

## 15. A base filename should exactly match the name of its default export.

```jsx
// file 1 contents
class CheckBox {
  // ...
}
export default CheckBox;

// file 2 contents
export default function fortyTwo() { return 42; }

// file 3 contents
export default function insideDirectory() {}

// in some other file
// bad
import CheckBox from './checkBox'; // PascalCase import/export, camelCase filename
import FortyTwo from './FortyTwo'; // PascalCase import/filename, camelCase export
import InsideDirectory from './InsideDirectory'; // PascalCase import/filename, camelCase export

// bad
import CheckBox from './check_box'; // PascalCase import/export, snake_case filename
import forty_two from './forty_two'; // snake_case import/filename, camelCase export
import inside_directory from './inside_directory'; // snake_case import, camelCase export
import index from './inside_directory/index'; // requiring the index file explicitly
import insideDirectory from './insideDirectory/index'; // requiring the index file explicitly

// good
import CheckBox from './CheckBox'; // PascalCase export/import/filename
import fortyTwo from './fortyTwo'; // camelCase export/import/filename
import insideDirectory from './insideDirectory'; // camelCase export/import/directory name/implicit "index"
// ^ supports both insideDirectory.js and insideDirectory/index.js
```

## 16. You may optionally uppercase a constant only 
If it (1) is exported, (2) is a const (it can not be reassigned), and (3) the programmer can trust it (and its nested properties) to never change.
Why? This is an additional tool to assist in situations where the programmer would be unsure if a variable might ever change. UPPERCASE_VARIABLES are letting the programmer know that they can trust the variable (and its properties) not to change.

What about all const variables? - This is unnecessary, so uppercasing should not be used for constants within a file. It should be used for exported constants, however.

What about exported objects? - Uppercase at the top level of export (e.g. EXPORTED_OBJECT.key) and maintain that all nested properties do not change.

```jsx
// bad
const PRIVATE_VARIABLE = 'should not be unnecessarily uppercased within a file';

// bad
export const THING_TO_BE_CHANGED = 'should obviously not be uppercased';

// bad
export let REASSIGNABLE_VARIABLE = 'do not use let with uppercase variables';

// ---

// allowed but does not supply semantic value
export const apiKey = 'SOMEKEY';

// better in most cases
export const API_KEY = 'SOMEKEY';

// ---

// bad - unnecessarily uppercases key while adding no semantic value
export const MAPPING = {
  KEY: 'value'
};

// good
export const MAPPING = {
  key: 'value'
};
```

## 17. Acronyms and initialisms should always be all uppercase or all lowercase.
Why? Names are for readability, not to appease a computer algorithm.

```jsx
// bad
import SmsContainer from './containers/SmsContainer';

// bad
const HttpRequests = [
  // ...
];

// good
import SMSContainer from './containers/SMSContainer';

// good
const HTTPRequests = [
  // ...
];

// also good
const httpRequests = [
  // ...
];

// best
import TextMessageContainer from './containers/TextMessageContainer';

// best
const requests = [
  // ...
];
```

## 18. Use Number.isNaN instead of global isNaN. eslint: no-restricted-globals
Why? The global isNaN coerces non-numbers to numbers, returning true for anything that coerces to NaN. If this behavior is desired, make it explicit.

```jsx
// bad
isNaN('1.2'); // false
isNaN('1.2.3'); // true

// good
Number.isNaN('1.2.3'); // false
Number.isNaN(Number('1.2.3')); // true
```

## 19. Never mutate parameters. eslint: no-param-reassign

```jsx
// bad
function f1(obj) {
  obj.key = 1;
}

// good
function f2(obj) {
  const key = obj?.key;
}
```
very cool with a smile

## 15. Avoid magic numbers. eslint: no-magic-numbers

```jsx
// bad 
...
if (text.lenght < 😎
...

// good
const maxLenghtPassword = 8;
...
if (text.lenght < maxLenghtPassword)
...

// bad
...
<p style={{marginTop: safeAreaTop + 16}}>Title</p>
...

// good
const marginTitleTop = 16
...
<p style={{marginTop: safeAreaTop + marginTitleTop}}>Title</p>
...

// good
const marginTitleTop = 16
const marginTitleTopWithSafeArea = safeAreaTop + marginTitleTop
...
<p style={{marginTop: marginTitleTopWithSafeArea}}>Title</p>
...
 ```

## React/ReactNative

## 1. Use PascalCase 
When you export a React Component and constructor / class / singleton / function library / bare object.

```jsx
function SomeReactComponent() {
  // ...
};

export default SomeReactComponent;
```

## 2. Always use camelCase for prop names

if the prop value is a React component.

```jsx
// bad
<Foo
  UserName="hello"
  phone_number={12345678}
/>

// good
<Foo
  userName="hello"
  phoneNumber={12345678}
  Component={SomeComponent}
/>
```

## 3. For all react components use function declaration instead of arrow functions.
It is not a big difference but it's still better to use classic function declaration for components to: 

avoid some re-renders using some libraries (React Navigation, etc.) 

automatic assignment of component "displayName" for debugging

```jsx
// bad
const SomeReactComponent = () => {
  // ...
};

SomeReactComponent.displayName = 'SomeReactComponent'

export default SomeReactComponent;

// good
function SomeReactComponent() {
  // ...
};

export default SomeReactComponent;

// best
export default function SomeReactComponent() {
  // ...
};
```

## 4. Don’t use useSelector in redux to avoid re-renders
Explanation: How to Make useSelector Not a Disaster 

For more info about how use redux with a project see this document: State Manager 

## 5. Extract reusable logic into custom hooks
Examples & explanation: 

Advanced React Hooks: Creating custom reusable Hooks - LogRocket Blog  

Creating and using custom React Hooks to promote reusability of logics 

## 6. Use absolute path everywhere

```jsx
// bad
import ... from './folder/lib'
import ... from '../folder/lib'
import ... from '../../folder/lib'

// good
import ... from 'root/folder/lib'
```

## 7. Lock Dependencies
This will ensure that you don’t import breaking changes from the new versions of the library into your project.

```jsx
// bad
"dependencies": {
  "some-cool-library": "^0.4.2",
  ...
}

// good
"dependencies": {
  "some-cool-library": "0.4.2",
  ...
}
```

## 8. Don’t use callbacks in the JSX

```jsx
// bad
function Component() {
  const [state, setState] = useState(null);

  return <div onClick={() => setState(...)}/>
}

// good
function Component() {
  const [state, setState] = useState(null);
  
  const handleClick = () => setState(...)

  return <div onClick={handleClick}/>
}

// bad
function Component() {
  return (
    <ul>
      {list.map((name) => <li>{name}</li>)}
    </ul>
  )
}

// good
function Component() {
  const renderItem = (name) => <li>{name}</li>
  
  return (
    <ul>
      {list.map(renderItem)}
    </ul>
  )
}
```

## 9. Keep your key prop unique across your whole app
Using the key prop is important because it helps React identify the exact element that has changed, is added, or is removed.

```jsx
// bad
function UserList() {
  const renderUserItem = (user) => <li>{user.name}</li>
  
  return userlist.map(renderUserItem)
}

// very bad
function UserList() {
  const renderUserItem = (user, index) => (
    <li key={index}>{user.name}</li>
  )
  
  return userlist.map(renderUserItem)
}

// good
function UserList() {
  const renderUserItem = (user) => <li key={user.id}>{user.name}</li>
  
  return userlist.map(renderUserItem)
}
```
But if it has not id param, so you can use an external library like nanoid for generating unique id's.

## 10. Use shorthand for boolean props

```jsx
// bad
<Form hasValidation={true} withErrors={true} />

// good
<Form hasValidation withErrors />
```

## 11. Avoid curly braces 
For string props and always use double quotes (") for JSX attributes, but single quotes (') for all other JS. eslint: jsx-quotes
Regular HTML attributes also typically use double quotes instead of single, so JSX attributes mirror this convention.

```jsx
// bad
<Paragraph variant={"h5"} />

// bad
<Paragraph variant='h5' />

// good
<Paragraph variant="h5" />
```

## 12. Use logical && operator instead of inline If in JSX in the next cases

```jsx
// bad 
<div>
  <h1>Hello!</h1>
  {
    unreadMessages.length > 0 ? (
      <h2>
        You have {unreadMessages.length} unread messages.
      </h2>
    ) : null
  }
</div>

// good
<div>
  <h1>Hello!</h1>
  {
    unreadMessages.length > 0 &&
      <h2>
        You have {unreadMessages.length} unread messages.
      </h2>
  }
</div>
```

## 13. Use the following order of code organization in the component file:


imports
constants
component
helpers
screens


# React Native

## 1. Use Pressable 
For touchable components instead of  TouchableOpacity || TouchableHighlight to avoid some UI cross-platform bugs and get to access a lot of new possibilities
Pressable · React Native 

## 2. Use .svg format 
For icons and small vector images for size reduction and more control.
Memory/CPU consumption investigation | Images & icons 

## 3. Navigation and route props
The priority is to use useNavigation and useRoute instead of navigation and route props

```jsx
// bad
function Component({navigation, route}) {
  const { userName } = route.params || {};

  const handlePress = () => navigation.navigate('UserScreen');

  return (
    <Pressable onPress={handlePress}>
      <Text>{userName}</Text>
    </Pressable
  )
}

//good
function Component() {
  const { navigate } = useNavigation();
  const { userName } = useRoute().params || {};

  const handlePress = () => navigate('UserScreen');

  return (
    <Pressable onPress={handlePress}>
      <Text>{userName}</Text>
    </Pressable
  )
}
```

## 4. Use StyleSheet.flatten 
In child components if they have self styles and the style prop is an array

```jsx
const styles = StyleSheet.create({
  title: { fontSize: 32 }
})

// bad
function Title({style, value}) {
  retrun <Text style={[styles.title, style]} >{value}<Text/>
}

// good
function Title({style, value}) {
  retrun <Text style={StyleSheet.flatten([styles.title, style])} >{value}<Text/>
}
```

## 5. Avoid Inline Styles

```jsx
// bad
function Title({style, value}) {
  const insets = useSafeAreaInsets()

  retrun (
    <Text 
      style={{
        fontFamily: 'some_font'
        fontSize: 32,
        marginTop: insets.top + 15,
      }}
    >
      {value}
    <Text/>
  )
}

// good
const TITLE_PADDING_TOP = 15

function Title({style, value}) {
  const insets = useSafeAreaInsets() 
  
  retrun (
    <Text 
      style={[
        styles.title,
        {marginTop: insets.top + TITLE_PADDING_TOP}
      ]} 
    >
      {value}
    <Text/>
  )
}
const styles = StyleSheet.create({
  title: { 
    fontFamily: 'some_font',
    fontSize: 32,
  }
})
```



![JS Camp](/img/app.jpg)

## Contributors ✨

Thanks goes to these wonderful people ([emoji key](https://allcontributors.org/docs/en/emoji-key)):

<table>
  <tr>
    <td align="center"><a href="https://fullstackserverless.github.io/"><img src="https://avatars0.githubusercontent.com/u/6774813?v=4?s=200" width="200px;" alt=""/><br /><sub><b>Dmitriy Vasilev</b></sub></a><br /> <a href="https://github.com/gHashTag/react-native-village/commits?author=gHashTag" title="Documentation">📖💲</a></td>
  </tr>
</table>

[![Become a Patron!](/img/logo/patreon.jpg)](https://www.patreon.com/bePatron?u=31769291)
