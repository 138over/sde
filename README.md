### Workspace Orchestration Services
```
install brew https://brew.sh

% brew install go
% brew install go-task/tap/go-task
% task -l
task: Available tasks for this project:
* build:                go build main.go
* lint:                 golint $(go list ./...)
* run:svc:web:          go run main.go service --name web
* run:svc:workspace:    go run main.go service --name workspace
* test:                 run unit, functional and system test
* test:functional:      functional test 
* test:system:          system test
* test:unit:            unit test
* utils:                download and install go utilities
```

### Experimentation with FaaS
```
Drive service configuration from external source, where handlers can live anywhere. 
Within the code, as a web service, or as a function (FaaS)

Handlers are candidates to live anywhere, whether that be state, error handling, or 
any non-DOM tasks
```

### Experimentation with Inline event configuration
```
% task build run:svc:web

open browser http://127.0.0.1:3000
open developer tools to observe console log
click Start Lifecycle

class ServiceStructure {
    constructor(route) {
        this._route = route;
    }

    static configure(route) {
        Lifecycle.createProcess('run-flow', ServiceLifecycleHandler)
            .addTask({
                description     : 'User clicked start flow button',
                eventType       : 'dom',
                onEvent         : 'click',
                selector        : '#arbitrary-flow',
                triggerEvent    : 'start:flow' 
            })
            .addTask({ 
                description     : "Run flow configuration",
                eventType       : 'app',
                onEvent         : 'start:flow',
                handler         : 'configureFlow',
                params          : { variant: 'release' },
                publishEvent    : 'flow:configured',
                mode            : 'debug'
            })
            .addTask({ 
                description     : "Create flow",
                eventType       : 'app',
                onEvent         : 'flow:configured',
                handler         : 'createFlow',
                params          : { variant: 'release' },
                publishEvent    : 'flow:created',
                mode            : 'debug'
            })
            .addTask({ 
                description     : "Run flow",
                eventType       : 'app',
                onEvent         : 'flow:created',
                handler         : 'runFlow',
                params          : { variant: 'release' },
                publishEvent    : 'flow:completed',
                mode            : 'debug'
            });

        return new ServiceStructure(route)
    }
}

```

### Experimentation with External event configuration
```
task build run:svc:web
open browser http://127.0.0.1:3000/lifecycle

TODO: implement method to read configuration from http://127.0.0.1:3000/lifecycle
      and dynamically create the processes for each name lifecycle
```

### Experimentation with DAGs and Svelte
[D3-DAG](https://github.com/erikbrinkman/d3-dag)  
[Svelte D3 Example](https://svelte.dev/repl/01a5774b53e9416584428c025668407b?version=3.15.0)  
```
TODO: implement adapter to generate data that works with d3-dag format
```

