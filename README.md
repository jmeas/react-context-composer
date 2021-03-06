# react-context-composor

React is proposing a new [https://github.com/reactjs/rfcs/pull/2](Context API). The API encourages composing. This utility component helps keep your code clean when your component will be rendering multiple Context Providers and Consumers.

## Install

```bash
npm install --save react-context-composor
```

## Usage

```jsx
import React, { Component } from 'react'
import ContextComposer from 'react-context-composer';
import createReactContext, { type Context } from 'create-react-context';

type Theme = 'light' | 'dark';
type Language = 'en';

// Pass a default theme to ensure type correctness
const ThemeContext: Context<Theme> = createReactContext('light');
const LanguageContext: Context<Language> = createReactContext('en');

class ThemeToggler extends React.Component<
  { children: Node },
  { theme: Theme, language: Language }
> {
  state = { theme: 'light', language: 'en' };
  render() {
    return (
      // Pass the current context value to the Provider's `value` prop.
      // Changes are detected using strict comparison (Object.is)
      <ContextComposer contexts={[
        <ThemeContext.Provider value={this.state.theme}>
        <LanguageContext.Provider value={this.state.language}>
      ]}>
        <button
          onClick={() => {
            this.setState(state => ({
              theme: state.theme === 'light' ? 'dark' : 'light'
            }));
          }}
        >
          Toggle theme
        </button>
        {this.props.children}
      </ThemeContext.Provider>
    );
  }
}

function Title({children}) {
  return (
    <ContextComposer contexts={[ThemeContext, LanguageContext]}>
      {(theme, language) => (
        <h1 style={{ color: theme === 'light' ? '#000' : '#fff' }}>
          {language}: {this.props.children}
        </h1>
      )}
    </ContextComposer>
  );
}

render(
  <ThemeToggler>
    <Title>Hi!</Title>
  </ThemeToggler>,
  domNode
);
```

## License

MIT © [Blaine Kasten](https://github.com/blainekasten)
