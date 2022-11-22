# dhtml-diagram-issues
# 1. lines between shaps overlap -- fixed after upgrade to version 5.0.0
## code snapt
```js
const diagram = new Diagram('diagram', {
      scale: this.scale,
      select: true,
      lineConfig: {
        lineGap: 50
      },
      autoplacement: {
        mode: 'edges',
        placeMode: 'radial'
      }
    });

    // add shape
    diagram.addShape('template', {
      template: this.generateCardTemple
    });

    diagram.data.parse(this.props.data);
    diagram.autoPlace();

    // attaching a handler to the event
    diagram.events.on('itemClick', (id) => {
      const request = this.findRequestInfo(id);
      if (!request) {
        return;
      }
      this.props.getRequestDetail(request.id, request.entityType).then((resp) => {
        this.setState({ showRequestDetail: true, requestDetail: resp });
      });
    });

    diagram.events.on('emptyAreaClick', () => {
      this.hideRequestDetail();
    });
```

**card template**

```js
function CardTemplate(props) {
  const { name, entityType, showNumber, number } = props;
  const className =
    'apm-diagram-card-template ' +
    (ENTITY_TYPE_APPLICATION === entityType ? 'application' : 'process');
  return (
    <div className={className}>
      <span>{name}</span>
      {showNumber ? <div className="corner-number">[{number}]</div> : null}
    </div>
  );
}
```

**data json**

```json
[
    {
        "type": "template",
        "entityType": "APPLICATION",
        "id": 30266,
        "text": "199 YYD test App 1",
        "downstream": [],
        "upstream": []
    },
    {
        "type": "template",
        "entityType": "APPLICATION",
        "id": 30267,
        "text": "199 YYD test App 3",
        "downstream": [
            30154,
            30152,
            30153
        ],
        "upstream": [
            30265,
            30268
        ]
    },
    {
        "type": "template",
        "entityType": "APPLICATION",
        "id": 30265,
        "text": "199 YYD test App 2",
        "downstream": [
            30267
        ],
        "upstream": [
            30266
        ]
    },
    {
        "type": "template",
        "entityType": "DEPENDENT_APPLICATION",
        "id": 30152,
        "text": "192 TestApp 2",
        "downstream": [
            30154
        ],
        "upstream": [
            30153
        ]
    },
    {
        "type": "template",
        "entityType": "DEPENDENT_APPLICATION",
        "id": 30153,
        "text": "192 TestApp 1",
        "downstream": [
            30152
        ],
        "upstream": [
            30266
        ]
    },
    {
        "type": "template",
        "entityType": "DEPENDENT_APPLICATION",
        "id": 30154,
        "text": "192 TestApp 3",
        "downstream": [],
        "upstream": [
            30152
        ]
    },
    {
        "type": "template",
        "entityType": "DEPENDENT_APPLICATION",
        "id": 30268,
        "text": "test2",
        "downstream": [],
        "upstream": []
    },
    {
        "forwardArrow": "filled",
        "from": 30267,
        "to": 30154
    },
    {
        "forwardArrow": "filled",
        "from": 30267,
        "to": 30152
    },
    {
        "forwardArrow": "filled",
        "from": 30267,
        "to": 30153
    },
    {
        "forwardArrow": "filled",
        "from": 30265,
        "to": 30267
    },
    {
        "forwardArrow": "filled",
        "from": 30268,
        "to": 30267
    },
    {
        "forwardArrow": "filled",
        "from": 30265,
        "to": 30267
    },
    {
        "forwardArrow": "filled",
        "from": 30266,
        "to": 30265
    },
    {
        "forwardArrow": "filled",
        "from": 30152,
        "to": 30154
    },
    {
        "forwardArrow": "filled",
        "from": 30153,
        "to": 30152
    },
    {
        "forwardArrow": "filled",
        "from": 30153,
        "to": 30152
    },
    {
        "forwardArrow": "filled",
        "from": 30266,
        "to": 30153
    },
    {
        "forwardArrow": "filled",
        "from": 30152,
        "to": 30154
    }
]
```
## differences using "dhtmlx-diagram-enterprise": "4.1.0" and libary from ../codebase/diagram.js?v=5.0.0
### from diagram-enterprise 4.1.0 diagram
![image](https://user-images.githubusercontent.com/24218496/197916767-ef67e50d-05be-47d6-b651-28ba9b80c914.png)
### from official diagram.js?v=5.0.0 libary (github https://github.com/DHTMLX/react-diagram-demo)
![image](https://user-images.githubusercontent.com/24218496/197916834-970f8a41-83be-46b0-bfd3-362402cbba36.png)

# 2.Why diagram scale turns to be 1 whatever I configured? Because of that, I can't zoom in or zoom out.
## render towice
This function in diagram.js from your js libary called twice, first time scale is actually I configured, but the second time it became to be 1
```js
 Diagram.prototype._render = function (vm) {
        var _this = this;
        var _a, _b;
        if (this._doNotRepaint && vm.node) {
            return vm.node;
        }
        var _c = this._getContent(), size = _c.size, svgContent = _c.svgContent, htmlContent = _c.htmlContent;
        this.events.fire(types_1.DiagramEvents.beforeRender, [size]);
        this._diagramSize = size;
        var _d = this.config, margin = _d.margin, type = _d.type, scale = _d.scale;
       ...
```
## code snapt
```js
const diagram = new Diagram('diagram', {
      scale: 2,
      select: true,
      lineConfig: {
        lineGap: 50
      },
      autoplacement: {
        mode: 'edges',
        placeMode: 'radial'
      }
    });

    // add shape
    diagram.addShape('template', {
      template: this.generateCardTemple
    });

    diagram.data.parse(this.props.data);
    diagram.autoPlace();

    // attaching a handler to the event
    diagram.events.on('itemClick', (id) => {
      const request = this.findRequestInfo(id);
      if (!request) {
        return;
      }
      this.props.getRequestDetail(request.id, request.entityType).then((resp) => {
        this.setState({ showRequestDetail: true, requestDetail: resp });
      });
    });

    diagram.events.on('emptyAreaClick', () => {
      this.hideRequestDetail();
    });
```

**card template**

```js
function CardTemplate(props) {
  const { name, entityType, showNumber, number } = props;
  const className =
    'apm-diagram-card-template ' +
    (ENTITY_TYPE_APPLICATION === entityType ? 'application' : 'process');
  return (
    <div className={className}>
      <span>{name}</span>
      {showNumber ? <div className="corner-number">[{number}]</div> : null}
    </div>
  );
}
```

**data json**

```json
[
    {
        "type": "template",
        "entityType": "APPLICATION",
        "id": 30266,
        "text": "199 YYD test App 1",
        "downstream": [],
        "upstream": []
    },
    {
        "type": "template",
        "entityType": "APPLICATION",
        "id": 30267,
        "text": "199 YYD test App 3",
        "downstream": [
            30154,
            30152,
            30153
        ],
        "upstream": [
            30265,
            30268
        ]
    },
    {
        "type": "template",
        "entityType": "APPLICATION",
        "id": 30265,
        "text": "199 YYD test App 2",
        "downstream": [
            30267
        ],
        "upstream": [
            30266
        ]
    },
    {
        "type": "template",
        "entityType": "DEPENDENT_APPLICATION",
        "id": 30152,
        "text": "192 TestApp 2",
        "downstream": [
            30154
        ],
        "upstream": [
            30153
        ]
    },
    {
        "type": "template",
        "entityType": "DEPENDENT_APPLICATION",
        "id": 30153,
        "text": "192 TestApp 1",
        "downstream": [
            30152
        ],
        "upstream": [
            30266
        ]
    },
    {
        "type": "template",
        "entityType": "DEPENDENT_APPLICATION",
        "id": 30154,
        "text": "192 TestApp 3",
        "downstream": [],
        "upstream": [
            30152
        ]
    },
    {
        "type": "template",
        "entityType": "DEPENDENT_APPLICATION",
        "id": 30268,
        "text": "test2",
        "downstream": [],
        "upstream": []
    },
    {
        "forwardArrow": "filled",
        "from": 30267,
        "to": 30154
    },
    {
        "forwardArrow": "filled",
        "from": 30267,
        "to": 30152
    },
    {
        "forwardArrow": "filled",
        "from": 30267,
        "to": 30153
    },
    {
        "forwardArrow": "filled",
        "from": 30265,
        "to": 30267
    },
    {
        "forwardArrow": "filled",
        "from": 30268,
        "to": 30267
    },
    {
        "forwardArrow": "filled",
        "from": 30265,
        "to": 30267
    },
    {
        "forwardArrow": "filled",
        "from": 30266,
        "to": 30265
    },
    {
        "forwardArrow": "filled",
        "from": 30152,
        "to": 30154
    },
    {
        "forwardArrow": "filled",
        "from": 30153,
        "to": 30152
    },
    {
        "forwardArrow": "filled",
        "from": 30153,
        "to": 30152
    },
    {
        "forwardArrow": "filled",
        "from": 30266,
        "to": 30153
    },
    {
        "forwardArrow": "filled",
        "from": 30152,
        "to": 30154
    }
]
```
![image](https://user-images.githubusercontent.com/24218496/203199013-432a1ac7-9f1d-4041-94ff-a6623bb834de.png)

