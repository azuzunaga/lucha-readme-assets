## Creating new activies
The application supports activity and statistics tracking via user input. Distance, elevation, and GPS coordinates from previously created routes can be logged by simply clicking on the route card.

![Creating new activities](https://github.com/azuzunaga/lucha-readme-assets/blob/master/new_activity.gif)


Route statistics are filled when a route is clicked. The route card component is reused throughout the application, so a prop is used to tell the component to add an event handler to each card:

```js
<ul className="route-detail-cards">
  {routes.reverse().map(route =>
    <RouteDetailContainer key={route.id} route={route} clickHandler={true} />
  )}
</ul>
```

Since route id information lives in separate components a new slice of state was added to handle the click events and combined with the UI slice of state in the Redux store: 

```js
const RouteIdReducer = (state = {}, action) => {
  Object.freeze(state);

  switch (action.type) {
    case RECEIVE_ROUTE_ID:
      return action.routeId;
    case CLEAR_ROUTE_ID:
      return {};
    default:
     return state;
  }
};
```

```js
import { combineReducers } from 'redux';

import RouteIdReducer from './route_id_reducer';
import userStatsReducer from './user_stats_reducer';

const uiReducer = combineReducers({
  routeId: RouteIdReducer,
  stats: userStatsReducer,
});

export default uiReducer;
```

Finally, inside of the NewActivity component the new global state is added to the component state using `componentWillReceiveProps`. On props change the state will update, triggering form re-render:

```js
componentWillReceiveProps(nextProps) {
  if (nextProps.routeId > 0 ) {
    const routeId = nextProps.routeId;
    const route = nextProps.routes[routeId];
    const distance = route.distance.toFixed(2);
    const elevation = route.elevation.toFixed(0);

    this.setState({
      polyline: route.polyline,
      big_image_url: route.bigImageUrl,
      distance: distance,
      elevation: elevation
    });
  }
}
```




