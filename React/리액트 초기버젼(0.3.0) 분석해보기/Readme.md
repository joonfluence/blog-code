# 구성방식

## 외부함수

```java
// 외부함수 (1)
const react = (function (e) {
  if ("function" == typeof bootstrap) bootstrap("react", e);
  else if ("object" == typeof exports) module.exports = e();
  else if ("function" == typeof define && define.amd) define(e);
  else if ("undefined" != typeof ses) {
    if (!ses.ok()) return;
    ses.makeReact = e;
  } else
    "undefined" != typeof window ? (window.React = e()) : (global.React = e());
// 외부함수 (2)
})(function () {
  var define, ses, bootstrap, module, exports;
  return (
    // 내부함수
  )(1); // react 모듈 실행
})
```

클로저로 복잡하게 사용됨. 주요 메서드를 내부 함수로 넣고, 1번 메서드인 react를 즉시호출.

## 내부함수

각 메서드는 숫자로 분리하여, 9천줄의 코드가 68개의 함수로 정리됨.

```javascript
({
  ...,
  // 마지막 내부함수
  68: [
    function (require, module, exports) {
      "use strict";

      var isNumber = {
        fillOpacity: true,
        fontWeight: true,
        opacity: true,
        orphans: true,
        textDecoration: true,
        zIndex: true,
        zoom: true,
      };

      var CSSProperty = {
        isNumber: isNumber,
      };

      module.exports = CSSProperty;
    },
    {},
  ],
}
```

### React

```javascript
function (require, module, exports) {
  /**
   * @providesModule React
   */

  "use strict";

  var ReactCompositeComponent = require("./ReactCompositeComponent");
  var ReactComponent = require("./ReactComponent");
  var ReactDOM = require("./ReactDOM");
  var ReactMount = require("./ReactMount");

  var ReactDefaultInjection = require("./ReactDefaultInjection");

  ReactDefaultInjection.inject();

  var React = {
    DOM: ReactDOM,
    initializeTouchEvents: function (shouldUseTouch) {
      ReactMount.useTouchEvents = shouldUseTouch;
    },
    autoBind: ReactCompositeComponent.autoBind,
    createClass: ReactCompositeComponent.createClass,
    createComponentRenderer: ReactMount.createComponentRenderer,
    constructAndRenderComponent: ReactMount.constructAndRenderComponent,
    constructAndRenderComponentByID:
      ReactMount.constructAndRenderComponentByID,
    renderComponent: ReactMount.renderComponent,
    unmountAndReleaseReactRootNode:
      ReactMount.unmountAndReleaseReactRootNode,
    isValidComponent: ReactComponent.isValidComponent,
  };

  module.exports = React;
}
```

**ReactDOM**

- DOM

**ReactCompositeComponent**

- autoBind
- createClass

**ReactMount**

- initializeTouchEvents
- createComponentRenderer
- constructAndRenderComponent
- constructAndRenderComponentByID
- renderComponent
- unmountAndReleaseReactRootNode

**ReactComponent**

- isValidComponent

#### ReactCompositeComponent

#### 주요 메서드

- createClass : Constructor, Render.
- autoBind

```javascript
var ReactCompositeComponent = {
  LifeCycle: CompositeLifeCycle,
  Base: ReactCompositeComponentBase,

  /**
   * Creates a composite component class given a class specification.
   *
   * @param {object} spec Class specification (which must define `render`).
   * @return {function} Component constructor function.
   * @public
   */
  createClass: function (spec) {
    var Constructor = function (initialProps, children) {
      this.construct(initialProps, children);
    };
    Constructor.prototype = new ReactCompositeComponentBase();
    Constructor.prototype.constructor = Constructor;
    mixSpecIntoComponent(Constructor, spec);
    invariant(
      Constructor.prototype.render,
      "createClass(...): Class specification must implement a `render` method."
    );

    var ConvenienceConstructor = function (props, children) {
      return new Constructor(props, children);
    };
    ConvenienceConstructor.componentConstructor = Constructor;
    ConvenienceConstructor.originalSpec = spec;
    return ConvenienceConstructor;
  },

  /**
   * Marks the provided method to be automatically bound to the component.
   * This means the method's context will always be the component.
   *
   *   React.createClass({
   *     handleClick: React.autoBind(function() {
   *       this.setState({jumping: true});
   *     }),
   *     render: function() {
   *       return <a onClick={this.handleClick}>Jump</a>;
   *     }
   *   });
   *
   * @param {function} method Method to be bound.
   * @public
   */
  autoBind: function (method) {
    function unbound() {
      invariant(
        false,
        "React.autoBind(...): Attempted to invoke an auto-bound method that " +
          "was not correctly defined on the class specification."
      );
    }
    unbound.__reactAutoBind = method;
    return unbound;
  },
};
```

#### ReactDOM

```javascript
function (require, module, exports) {
  /**
   * @providesModule ReactDOM
   * @typechecks
   */

  "use strict";

  var ReactNativeComponent = require("./ReactNativeComponent");

  var mergeInto = require("./mergeInto");
  var objMapKeyVal = require("./objMapKeyVal");

  /**
   * Creates a new React class that is idempotent and capable of containing other
   * React components. It accepts event listeners and DOM properties that are
   * valid according to `DOMProperty`.
   *
   *  - Event listeners: `onClick`, `onMouseDown`, etc.
   *  - DOM properties: `className`, `name`, `title`, etc.
   *
   * The `style` property functions differently from the DOM API. It accepts an
   * object mapping of style properties to values.
   *
   * @param {string} tag Tag name (e.g. `div`).
   * @param {boolean} omitClose True if the close tag should be omitted.
   * @private
   */
  function createDOMComponentClass(tag, omitClose) {
    var Constructor = function (initialProps, children) {
      this.construct(initialProps, children);
    };

    Constructor.prototype = new ReactNativeComponent(tag, omitClose);
    Constructor.prototype.constructor = Constructor;

    return function (props, children) {
      return new Constructor(props, children);
    };
  }

  /**
   * Creates a mapping from supported HTML tags to `ReactNativeComponent` classes.
   * This is also accessible via `React.DOM`.
   *
   * @public
   */
  var ReactDOM = objMapKeyVal(
    {
      a: false,
      abbr: false,
      address: false,
      audio: false,
      b: false,
      body: false,
      br: true,
      button: false,
      code: false,
      col: true,
      colgroup: false,
      dd: false,
      div: false,
      section: false,
      dl: false,
      dt: false,
      em: false,
      embed: true,
      fieldset: false,
      footer: false,
      // Danger: this gets monkeypatched! See ReactDOMForm for more info.
      form: false,
      h1: false,
      h2: false,
      h3: false,
      h4: false,
      h5: false,
      h6: false,
      header: false,
      hr: true,
      i: false,
      iframe: false,
      img: true,
      input: true,
      label: false,
      legend: false,
      li: false,
      line: false,
      nav: false,
      object: false,
      ol: false,
      optgroup: false,
      option: false,
      p: false,
      param: true,
      pre: false,
      select: false,
      small: false,
      source: false,
      span: false,
      sub: false,
      sup: false,
      strong: false,
      table: false,
      tbody: false,
      td: false,
      textarea: false,
      tfoot: false,
      th: false,
      thead: false,
      time: false,
      title: false,
      tr: false,
      u: false,
      ul: false,
      video: false,
      wbr: false,

      // SVG
      circle: false,
      g: false,
      path: false,
      polyline: false,
      rect: false,
      svg: false,
      text: false,
    },
    createDOMComponentClass
  );

  var injection = {
    injectComponentClasses: function (componentClasses) {
      mergeInto(ReactDOM, componentClasses);
    },
  };

  ReactDOM.injection = injection;

  module.exports = ReactDOM;
}
```

objMapKeyVal 메서드로 HTML 태그 값을 매핑 후, 콜백함수로 createDOMComponentClass 실행.

#### ReactMount

```javascript
function (require, module, exports) {
  (function () {
    /**
     * @providesModule ReactMount
     */

    "use strict";

    var ReactEvent = require("./ReactEvent");
    var ReactInstanceHandles = require("./ReactInstanceHandles");
    var ReactEventTopLevelCallback = require("./ReactEventTopLevelCallback");

    var $ = require("./$");

    var globalMountPointCounter = 0;

    /** Mapping from reactRoot DOM ID to React component instance. */
    var instanceByReactRootID = {};

    /** Mapping from reactRoot DOM ID to `container` nodes. */
    var containersByReactRootID = {};

    /**
     * @param {DOMElement} container DOM element that may contain a React component.
     * @return {?string} A "reactRoot" ID, if a React component is rendered.
     */
    function getReactRootID(container) {
      return container.firstChild && container.firstChild.id;
    }

    /**
     * Mounting is the process of initializing a React component by creatings its
     * representative DOM elements and inserting them into a supplied `container`.
     * Any prior content inside `container` is destroyed in the process.
     *
     *   ReactMount.renderComponent(component, $('container'));
     *
     *   <div id="container">         <-- Supplied `container`.
     *     <div id=".reactRoot[3]">   <-- Rendered reactRoot of React component.
     *       // ...
     *     </div>
     *   </div>
     *
     * Inside of `container`, the first element rendered is the "reactRoot".
     */
    var ReactMount = {
      /** Time spent generating markup. */
      totalInstantiationTime: 0,

      /** Time spent inserting markup into the DOM. */
      totalInjectionTime: 0,

      /** Whether support for touch events should be initialized. */
      useTouchEvents: false,

      /**
       * This is a hook provided to support rendering React components while
       * ensuring that the apparent scroll position of its `container` does not
       * change.
       *
       * @param {DOMElement} container The `container` being rendered into.
       * @param {function} renderCallback This must be called once to do the render.
       */
      scrollMonitor: function (container, renderCallback) {
        renderCallback();
      },

      /**
       * Ensures tht the top-level event delegation listener is set up. This will be
       * invoked some time before the first time any React component is rendered.
       *
       * @param {object} TopLevelCallbackCreator
       * @private
       */
      prepareTopLevelEvents: function (TopLevelCallbackCreator) {
        ReactEvent.ensureListening(
          ReactMount.useTouchEvents,
          TopLevelCallbackCreator
        );
      },

      /**
       * Renders a React component into the DOM in the supplied `container`.
       *
       * If the React component was previously rendered into `container`, this will
       * perform an update on it and only mutate the DOM as necessary to reflect the
       * latest React component.
       *
       * @param {ReactComponent} nextComponent Component instance to render.
       * @param {DOMElement} container DOM element to render into.
       * @return {ReactComponent} Component instance rendered in `container`.
       */
      renderComponent: function (nextComponent, container) {
        var prevComponent =
          instanceByReactRootID[getReactRootID(container)];
        if (prevComponent) {
          var nextProps = nextComponent.props;
          ReactMount.scrollMonitor(container, function () {
            prevComponent.replaceProps(nextProps);
          });
          return prevComponent;
        }

        ReactMount.prepareTopLevelEvents(ReactEventTopLevelCallback);

        var reactRootID = ReactMount.registerContainer(container);
        instanceByReactRootID[reactRootID] = nextComponent;
        nextComponent.mountComponentIntoNode(reactRootID, container);
        return nextComponent;
      },

      /**
       * Creates a function that accepts a `container` and renders the supplied
       * React component instance into it.
       *
       *   var renderInto = ReactMount.createComponentRenderer(component);
       *   // ...
       *   var component = renderInto($('container'));
       *
       * @param {ReactComponent} component Component instance to render.
       * @return {function(DOMElement): ReactComponent}
       */
      createComponentRenderer: function (component) {
        return function (container) {
          return ReactMount.renderComponent(component, container);
        };
      },

      /**
       * Constructs a component instance of `constructor` with `initialProps` and
       * renders it into the supplied `container`.
       *
       * @param {function} constructor React component constructor.
       * @param {?object} props Initial props of the component instance.
       * @param {DOMElement} container DOM element to render into.
       * @return {ReactComponent} Component instance rendered in `container`.
       */
      constructAndRenderComponent: function (
        constructor,
        props,
        container
      ) {
        return ReactMount.renderComponent(
          constructor(props),
          container
        );
      },

      /**
       * Constructs a component instance of `constructor` with `initialProps` and
       * renders it into a container node identified by supplied `id`.
       *
       * @param {function} componentConstructor React component constructor
       * @param {?object} props Initial props of the component instance.
       * @param {string} id ID of the DOM element to render into.
       * @return {ReactComponent} Component instance rendered in the container node.
       */
      constructAndRenderComponentByID: function (
        constructor,
        props,
        id
      ) {
        return ReactMount.constructAndRenderComponent(
          constructor,
          props,
          $(id)
        );
      },

      /**
       * Registers a container node into which React components will be rendered.
       * This also creates the "reatRoot" ID that will be assigned to the element
       * rendered within.
       *
       * @param {DOMElement} container DOM element to register as a container.
       * @return {string} The "reactRoot" ID of elements rendered within.
       */
      registerContainer: function (container) {
        var reactRootID = getReactRootID(container);
        if (reactRootID) {
          // If one exists, make sure it is a valid "reactRoot" ID.
          reactRootID =
            ReactInstanceHandles.getReactRootIDFromNodeID(reactRootID);
        }
        if (!reactRootID) {
          // No valid "reactRoot" ID found, create one.
          reactRootID = ReactInstanceHandles.getReactRootID(
            globalMountPointCounter++
          );
        }
        containersByReactRootID[reactRootID] = container;
        return reactRootID;
      },

      /**
       * Unmounts and destroys the React component rendered in the `container`.
       *
       * @param {DOMElement} container DOM element containing a React component.
       */
      unmountAndReleaseReactRootNode: function (container) {
        var reactRootID = getReactRootID(container);
        var component = instanceByReactRootID[reactRootID];
        // TODO: Consider throwing if no `component` was found.
        component.unmountComponentFromNode(container);
        delete instanceByReactRootID[reactRootID];
        delete containersByReactRootID[reactRootID];
      },

      /**
       * Finds the container DOM element that contains React component to which the
       * supplied DOM `id` belongs.
       *
       * @param {string} id The ID of an element rendered by a React component.
       * @return {?DOMElement} DOM element that contains the `id`.
       */
      findReactContainerForID: function (id) {
        var reatRootID =
          ReactInstanceHandles.getReactRootIDFromNodeID(id);
        // TODO: Consider throwing if `id` is not a valid React element ID.
        return containersByReactRootID[reatRootID];
      },

      /**
       * Given the ID of a DOM node rendered by a React component, finds the root
       * DOM node of the React component.
       *
       * @param {string} id ID of a DOM node in the React component.
       * @return {?DOMElement} Root DOM node of the React component.
       */
      findReactRenderedDOMNodeSlow: function (id) {
        var reactRoot = ReactMount.findReactContainerForID(id);
        return ReactInstanceHandles.findComponentRoot(reactRoot, id);
      },
    };

    module.exports = ReactMount;
  })();
}
```

#### ReactComponent

```javascript
function (require, module, exports) {
  /**
   * @providesModule ReactComponent
   */

  "use strict";

  var ExecutionEnvironment = require("./ExecutionEnvironment");
  var ReactCurrentOwner = require("./ReactCurrentOwner");
  var ReactDOMIDOperations = require("./ReactDOMIDOperations");
  var ReactMount = require("./ReactMount");
  var ReactOwner = require("./ReactOwner");
  var ReactReconcileTransaction = require("./ReactReconcileTransaction");

  var invariant = require("./invariant");
  var keyMirror = require("./keyMirror");
  var merge = require("./merge");

  /**
   * Prop key that references a component's owner.
   * @private
   */
  var OWNER = "{owner}";

  /**
   * Every React component is in one of these life cycles.
   */
  var ComponentLifeCycle = keyMirror({
    /**
     * Mounted components have a DOM node representation and are capable of
     * receiving new props.
     */
    MOUNTED: null,
    /**
     * Unmounted components are inactive and cannot receive new props.
     */
    UNMOUNTED: null,
  });

  /**
   * Components are the basic units of composition in React.
   *
   * Every component accepts a set of keyed input parameters known as "props" that
   * are initialized by the constructor. Once a component is mounted, the props
   * can be mutated using `setProps` or `replaceProps`.
   *
   * Every component is capable of the following operations:
   *
   *   `mountComponent`
   *     Initializes the component, renders markup, and registers event listeners.
   *
   *   `receiveProps`
   *     Updates the rendered DOM nodes given a new set of props.
   *
   *   `unmountComponent`
   *     Releases any resources allocated by this component.
   *
   * Components can also be "owned" by other components. Being owned by another
   * component means being constructed by that component. This is different from
   * being the child of a component, which means having a DOM representation that
   * is a child of the DOM representation of that component.
   *
   * @class ReactComponent
   */
  var ReactComponent = {
    /**
     * @param {?object} object
     * @return {boolean} True if `object` is a valid component.
     * @final
     */
    isValidComponent: function (object) {
      return !!(
        object &&
        typeof object.mountComponentIntoNode === "function" &&
        typeof object.receiveProps === "function"
      );
    },

    /**
     * @internal
     */
    LifeCycle: ComponentLifeCycle,

    /**
     * React references `ReactDOMIDOperations` using this property in order to
     * allow dependency injection.
     *
     * @internal
     */
    DOMIDOperations: ReactDOMIDOperations,

    /**
     * React references `ReactReconcileTransaction` using this property in order
     * to allow dependency injection.
     *
     * @internal
     */
    ReactReconcileTransaction: ReactReconcileTransaction,

    /**
     * @param {object} DOMIDOperations
     * @final
     */
    setDOMOperations: function (DOMIDOperations) {
      ReactComponent.DOMIDOperations = DOMIDOperations;
    },

    /**
     * @param {Transaction} ReactReconcileTransaction
     * @final
     */
    setReactReconcileTransaction: function (ReactReconcileTransaction) {
      ReactComponent.ReactReconcileTransaction =
        ReactReconcileTransaction;
    },

    /**
     * Base functionality for every ReactComponent constructor.
     *
     * @lends {ReactComponent.prototype}
     */
    Mixin: {
      /**
       * Returns the DOM node rendered by this component.
       *
       * @return {?DOMElement} The root node of this component.
       * @final
       * @protected
       */
      getDOMNode: function () {
        invariant(
          ExecutionEnvironment.canUseDOM,
          "getDOMNode(): The DOM is not supported in the current environment."
        );
        invariant(
          this._lifeCycleState === ComponentLifeCycle.MOUNTED,
          "getDOMNode(): A component must be mounted to have a DOM node."
        );
        var rootNode = this._rootNode;
        if (!rootNode) {
          rootNode = document.getElementById(this._rootNodeID);
          if (!rootNode) {
            // TODO: Log the frequency that we reach this path.
            rootNode = ReactMount.findReactRenderedDOMNodeSlow(
              this._rootNodeID
            );
          }
          this._rootNode = rootNode;
        }
        return rootNode;
      },

      /**
       * Sets a subset of the props.
       *
       * @param {object} partialProps Subset of the next props.
       * @final
       * @public
       */
      setProps: function (partialProps) {
        this.replaceProps(merge(this.props, partialProps));
      },

      /**
       * Replaces all of the props.
       *
       * @param {object} props New props.
       * @final
       * @public
       */
      replaceProps: function (props) {
        invariant(
          !this.props[OWNER],
          "replaceProps(...): You called `setProps` or `replaceProps` on a " +
            "component with an owner. This is an anti-pattern since props will " +
            "get reactively updated when rendered. Instead, change the owner's " +
            "`render` method to pass the correct value as props to the component " +
            "where it is created."
        );
        var transaction =
          ReactComponent.ReactReconcileTransaction.getPooled();
        transaction.perform(
          this.receiveProps,
          this,
          props,
          transaction
        );
        ReactComponent.ReactReconcileTransaction.release(transaction);
      },

      /**
       * Base constructor for all React component.
       *
       * Subclasses that override this method should make sure to invoke
       * `ReactComponent.Mixin.construct.call(this, ...)`.
       *
       * @param {?object} initialProps
       * @param {*} children
       * @internal
       */
      construct: function (initialProps, children) {
        this.props = initialProps || {};
        if (typeof children !== "undefined") {
          this.props.children = children;
        }
        // Record the component responsible for creating this component.
        this.props[OWNER] = ReactCurrentOwner.current;
        // All components start unmounted.
        this._lifeCycleState = ComponentLifeCycle.UNMOUNTED;
      },

      /**
       * Initializes the component, renders markup, and registers event listeners.
       *
       * NOTE: This does not insert any nodes into the DOM.
       *
       * Subclasses that override this method should make sure to invoke
       * `ReactComponent.Mixin.mountComponent.call(this, ...)`.
       *
       * @param {string} rootID DOM ID of the root node.
       * @param {ReactReconcileTransaction} transaction
       * @return {?string} Rendered markup to be inserted into the DOM.
       * @internal
       */
      mountComponent: function (rootID, transaction) {
        invariant(
          this._lifeCycleState === ComponentLifeCycle.UNMOUNTED,
          "mountComponent(%s, ...): Can only mount an unmounted component.",
          rootID
        );
        var props = this.props;
        if (props.ref != null) {
          ReactOwner.addComponentAsRefTo(this, props.ref, props[OWNER]);
        }
        this._rootNodeID = rootID;
        this._lifeCycleState = ComponentLifeCycle.MOUNTED;
        // Effectively: return '';
      },

      /**
       * Releases any resources allocated by `mountComponent`.
       *
       * NOTE: This does not remove any nodes from the DOM.
       *
       * Subclasses that override this method should make sure to invoke
       * `ReactComponent.Mixin.unmountComponent.call(this)`.
       *
       * @internal
       */
      unmountComponent: function () {
        invariant(
          this._lifeCycleState === ComponentLifeCycle.MOUNTED,
          "unmountComponent(): Can only unmount a mounted component."
        );
        var props = this.props;
        if (props.ref != null) {
          ReactOwner.removeComponentAsRefFrom(
            this,
            props.ref,
            props[OWNER]
          );
        }
        this._rootNode = null;
        this._rootNodeID = null;
        this._lifeCycleState = ComponentLifeCycle.UNMOUNTED;
      },

      /**
       * Updates the rendered DOM nodes given a new set of props.
       *
       * Subclasses that override this method should make sure to invoke
       * `ReactComponent.Mixin.receiveProps.call(this, ...)`.
       *
       * @param {object} nextProps Next set of properties.
       * @param {ReactReconcileTransaction} transaction
       * @internal
       */
      receiveProps: function (nextProps, transaction) {
        invariant(
          this._lifeCycleState === ComponentLifeCycle.MOUNTED,
          "receiveProps(...): Can only update a mounted component."
        );
        var props = this.props;
        // If either the owner or a `ref` has changed, make sure the newest owner
        // has stored a reference to `this`, and the previous owner (if different)
        // has forgotten the reference to `this`.
        if (
          nextProps[OWNER] !== props[OWNER] ||
          nextProps.ref !== props.ref
        ) {
          if (props.ref != null) {
            ReactOwner.removeComponentAsRefFrom(
              this,
              props.ref,
              props[OWNER]
            );
          }
          // Correct, even if the owner is the same, and only the ref has changed.
          if (nextProps.ref != null) {
            ReactOwner.addComponentAsRefTo(
              this,
              nextProps.ref,
              nextProps[OWNER]
            );
          }
        }
      },

      /**
       * Mounts this component and inserts it into the DOM.
       *
       * @param {string} rootID DOM ID of the root node.
       * @param {DOMElement} container DOM element to mount into.
       * @final
       * @internal
       * @see {ReactMount.renderComponent}
       */
      mountComponentIntoNode: function (rootID, container) {
        var transaction =
          ReactComponent.ReactReconcileTransaction.getPooled();
        transaction.perform(
          this._mountComponentIntoNode,
          this,
          rootID,
          container,
          transaction
        );
        ReactComponent.ReactReconcileTransaction.release(transaction);
      },

      /**
       * @param {string} rootID DOM ID of the root node.
       * @param {DOMElement} container DOM element to mount into.
       * @param {ReactReconcileTransaction} transaction
       * @final
       * @private
       */
      _mountComponentIntoNode: function (
        rootID,
        container,
        transaction
      ) {
        var renderStart = Date.now();
        var markup = this.mountComponent(rootID, transaction);
        ReactMount.totalInstantiationTime += Date.now() - renderStart;

        var injectionStart = Date.now();
        // Asynchronously inject markup by ensuring that the container is not in
        // the document when settings its `innerHTML`.
        var parent = container.parentNode;
        if (parent) {
          var next = container.nextSibling;
          parent.removeChild(container);
          container.innerHTML = markup;
          if (next) {
            parent.insertBefore(container, next);
          } else {
            parent.appendChild(container);
          }
        } else {
          container.innerHTML = markup;
        }
        ReactMount.totalInjectionTime += Date.now() - injectionStart;
      },

      /**
       * Unmounts this component and removes it from the DOM.
       *
       * @param {DOMElement} container DOM element to unmount from.
       * @final
       * @internal
       * @see {ReactMount.unmountAndReleaseReactRootNode}
       */
      unmountComponentFromNode: function (container) {
        this.unmountComponent();
        // http://jsperf.com/emptying-a-node
        while (container.lastChild) {
          container.removeChild(container.lastChild);
        }
      },

      /**
       * Checks if this component is owned by the supplied `owner` component.
       *
       * @param {ReactComponent} owner Component to check.
       * @return {boolean} True if `owners` owns this component.
       * @final
       * @internal
       */
      isOwnedBy: function (owner) {
        return this.props[OWNER] === owner;
      },
    },
  };

  function logDeprecated(msg) {
    if (true) {
      throw new Error(msg);
    }
  }

  /**
   * @deprecated
   */
  ReactComponent.Mixin.update = function (props) {
    logDeprecated("this.update() is deprecated. Use this.setProps()");
    this.setProps(props);
  };
  ReactComponent.Mixin.updateAll = function (props) {
    logDeprecated(
      "this.updateAll() is deprecated. Use this.replaceProps()"
    );
    this.replaceProps(props);
  };

  module.exports = ReactComponent;
}
```

- isValidComponent
- setDOMOperations
- setReactReconcileTransaction
- getDOMNode
- setProps
- replaceProps
- construct
- mountComponent
- unmountComponent
- receiveProps
- mountComponentIntoNode
- isOwnedBy

### 기타

#### objMapKeyVal

```javascript
function (require, module, exports) {
  /**
   * @providesModule objMapKeyVal
   */

  "use strict";

  /**
   * Behaves the same as `objMap` but invokes func with the key first, and value
   * second. Use `objMap` unless you need this special case.
   * Invokes func as:
   *
   *   func(key, value, iteration)
   *
   * @param {?object} obj Object to map keys over
   * @param {!function} func Invoked for each key/val pair.
   * @param {?*} context
   * @return {?object} Result of mapping or null if obj is falsey
   */
  function objMapKeyVal(obj, func, context) {
    if (!obj) {
      return null;
    }
    var i = 0;
    var ret = {};
    for (var key in obj) {
      if (obj.hasOwnProperty(key)) {
        ret[key] = func.call(context, key, obj[key], i++);
      }
    }
    return ret;
  }

  module.exports = objMapKeyVal;
}
```

createDOMComponentClass 호출. 태그 컴포넌트 생성.
