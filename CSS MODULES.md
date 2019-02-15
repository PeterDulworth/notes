# CSS MODULES

CSS modulese provide each file with it's own "scope" by compiling classnames into unique classnames.

*"CSS Modules guarantee that all the styles for a single componen live in one place and only apply to that component and nothing else."*

### Installation

If you are using create-react-app then no installation required! 

### File Names

Use the file extension `module.css` instead of `.css` (`module.scss` for scss).

> Note: The extension should be `module.css ` exactly. `module` should not be replaced with the module name.

### Importing

```javascript
import btn from './Button.module.css';
```

You can use whatever word you makes the most sense in your context instead of `styles`.  i.e. it doesn't depend on the contents of `Button.module.css`.

### Using normal CSS vs. CSS Modules

#### CSS Modules

To reference the `.red` class in the `Button.module.css` stylesheet:

```javascript
import btn from './Button.module.css';

class Button extends Component<> {
    render() {
        return <button className={btn.red}></button>
    }
}
```

#### Normal CSS

```javascript
import './Button.css';

class Button extends Component<> {
    render() {
        return <button className={red}></button>
    }
}
```

### Learn More

https://css-tricks.com/css-modules-part-1-need/

https://css-tricks.com/css-modules-part-2-getting-started/

https://css-tricks.com/css-modules-part-3-react/