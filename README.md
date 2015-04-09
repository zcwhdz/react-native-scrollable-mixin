# ScrollableMixin

ScrollableMixin lets your scrollable React Native components conform to a standard interface, making it easier to compose components. This lets you compose different types of ScrollView-like components while preserving the `ScrollView` API, including methods like `scrollTo`.

## Installation
```
npm install react-native-scrollable-mixin
```

## Usage

Add ScrollableMixin to your scrollable React components and implement `getScrollResponder()`, which must return the underlying scrollable component's scroll responder.

```js
var ScrollableMixin = require('react-native-scrollable-mixin');

var InfiniteScrollView = React.createClass({
  mixins: [ScrollableMixin],

  propTypes: {
    ...ScrollView.propTypes,
    scrollComponentClass: React.PropTypes.func.isRequired,
  },

  /**
   * IMPORTANT: You must return the scroll responder of the underlying
   * scrollable component from getScrollResponder() when using ScrollableMixin.
   */
  getScrollResponder() {
    return this._scrollView.getScrollResponder();
  },

  render() {
    var ScrollComponent = this.props.scrollViewClass;
    return (
      <ScrollComponent {...this.props} ref={(c) => this._scrollView = c}>
        {this.props.children}
      </ScrollComponent>
    );
  },
});
```

## Features

By mixing in ScrollableMixin, your custom component gets the `ScrollView` API. For example:

```js
class App extends React.Component {
  render() {
    return (
      <InfiniteScrollView
        ref={(c) => this._scrollView = c}
        scrollComponentClass={ListView}
        dataSource={...}
        renderRow={...}
      />
    );
  }

  _scrollToTop() {
    // By having all scrollable components conform to ScrollableMixin's
    // standard, calling `scrollTo` on your top-level scrollable component will
    // successfully scroll the underlying scroll view.
    this._scrollView.scrollTo(0, 0);
  }
}
```
