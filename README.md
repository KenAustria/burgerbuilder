# Burger Builder
<img src="src/assets/images/burger-logo.png">  

Ecommerce application for dynamically building burgers with the ability to create an order that is sent to Firebase. [Live Link](https://burgerbuilder-be355.firebaseapp.com/)

## Technologies

• React  
• React Router  
• Redux  
• Redux Thunk  
• Axios  
• Firebase Database  
• CSS Modules  

## Features

• Users can customize a burger to their liking.  
• Users can checkout with their order.  
• Users can view order history.

---

## Highlight Demonstrations

### ⭐ Users can customize a burger to their liking.

An empty burger is initiated upon visiting the application. The 'Order' button is disabled, along with each ingredient's decrementing build control to prompt the user to add ingredients to their custom burger.  

![](https://media.giphy.com/media/7OWvhNBmAKF1kiF3Ta/giphy.gif)  
  
  
The buildControls functional component maps through the controls array of objects and renders a BuildControl component per object.  
```
// BuildControl Component
const controls = [
  { label: 'Lettuce', type: 'salad' },
  { label: 'Bacon', type: 'bacon' },
  { label: 'Cheese', type: 'cheese' },
  { label: 'Meat', type: 'meat' }
];

const buildControls = props => (
  <div className={classes.BuildControls}>
    <p>
      Current Price: <strong>{props.price.toFixed(2)}</strong>
    </p>
    {controls.map(ctrl => (
      <BuildControl
        key={ctrl.label}
        label={ctrl.label}
        added={() => props.ingredientAdded(ctrl.type)}
        removed={() => props.ingredientRemoved(ctrl.type)}
        disabled={props.disabled[ctrl.type]}
      />
    ))}
    <button
      className={classes.OrderButton}
      disabled={!props.purchasable}
      onClick={props.ordered}
    >
      {props.isAuth ? "ORDER NOW" : "SIGN UP TO ORDER"}
    </button>
  </div>
);
```
The BurgerIngredient component expects information about which ingredient to render via props. The ingredient variable is initially set to null to not render an ingredient in the case something invalid is passed in.  

```
// BurgerIngredient
class BurgerIngredient extends Component {
  render () {
    let ingredient = null;

    switch ( this.props.type ) {
      case ( 'bread-bottom' ):
        ingredient = <div className={classes.BreadBottom}></div>;
        break;
      case ( 'bread-top' ):
        ingredient = (
          <div className={classes.BreadTop}>
            <div className={classes.Seeds1}></div>
            <div className={classes.Seeds2}></div>
          </div>
        );
        break;
      case ( 'meat' ):
        ingredient = <div className={classes.Meat}></div>;
        break;
      case ( 'cheese' ):
        ingredient = <div className={classes.Cheese}></div>;
        break;
      case ( 'bacon' ):
        ingredient = <div className={classes.Bacon}></div>;
        break;
      case ( 'salad' ):
        ingredient = <div className={classes.Salad}></div>;
        break;
      default:
        ingredient = null;
    }

    return ingredient;
  }
}
```

### ⭐ Users can checkout with their order and view their order history.

The orderHandler accesses the ingredients used and contact data of the user. We declare the formData host object that will take each formElementIdentifier of orderForm to assign to itself. The formData object will be assigned to the 'order' object, along with the ingredients and total price. The onOrderBurger action is then trigerred with the 'order' object and user's authenticated token passed in.  

![](https://media.giphy.com/media/2fuQuR500VXpdOdqeU/giphy.gif)

```
orderHandler = event => {
  event.preventDefault();

  const formData = {};
  for (let formElementIdentifier in this.state.orderForm) {
    formData[formElementIdentifier] = this.state.orderForm[
      formElementIdentifier
    ].value;
  }
  const order = {
    ingredients: this.props.ings,
    price: this.props.price,
    orderData: formData
  };

  this.props.onOrderBurger(order, this.props.token);
};
```

## Roadmap

React Hooks
