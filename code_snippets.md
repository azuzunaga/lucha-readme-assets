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

```js
<ul className="route-detail-cards">
  {routes.reverse().map(route =>
    <RouteDetailContainer key={route.id} route={route} clickHandler={true} />
  )}
</ul>
```


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

