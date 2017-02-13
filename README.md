# vuejs-countdown
Example:
```javascript
<countdown :start="startDate" :component-class="'countdown'" :countdown="timeout" @deadline-reached="deleteOrder()"></countdown>
```

Props:
- **start** - Start date property
- **component-class** - Assign specified class to component's main div
- **countdown** - Amount of minutes after which execute deadline-reached callback

Events:
- **deadline-reached** - Launched when timeout value becames 0
