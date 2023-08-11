You want to build a Copilot for your product.  Time to buckle in!

## The Challenge
Building a copilot is a non-trivial endeavor. There are three primary challenges that all developers face when building a copilot. These are exposing platform access, planning & execution of tasks, and steering.  

### Exposing Platform Access
The most fundamental problem with building a platform copilot is providing access to the platform.  There are really two ways of doing this.  Both rely on the usage of an abstraction that we call **Invokables**.

#### Invokables
Where open source platforms often use the tool abstraction to expose platform access, we use an abstraction we call invokables. The nuanced reasons we have chosen to go with another abstraction from tools are not discussed in detail here.  With that said, invokables subsume tools, so anything that can be done with tools can be done with invokables.  Invokables provide stronger guarantees and recovery paths than tools do.  These constraints allow the planning module of the copilot to create reliable paths.  Nonetheless, the base interface for an invokable is quite similar to a tool:

```typescript
interface Invokable {
	invoke: (...args: any[]) => string,
	name?: string,
	description?: string,
	schema?: z.object
}
```

Other invokables such as ConfirmInvokable, StartInvokable, and all others extend this interface to add additional functionality and or metadata.

#### Manual Function Exposing
The most natural and simplest way to expose functionality comes by exposing the invokable interface to the user in their codebase.  In typescript, we provide the following imports:

```typescript
import { Copilot, Invokable } from 'layer';
```

The `Copilot` is a React component that functions as a front-end interface for users, and a hook provider.  The basic `Invokable` class is the same as the one for the interface described above.  You can directly create an invokable with this approach, but it is often easier to use the convenience hook to do this as below.

```typescript
const App () => {

	const [title, setTitle] = useState("Enter Title")

	invokable({invoke: setTitle, description: "Used to set app title"})
	
	return (
		<div>
			<h1>{title}</h1>
			<Content />
		</div>
	)
}
```

So the above approach is fantastic if you have a React application or use Redux for state management.  The problem is that the above approach is not hugely scalable in every front-end architecture.
#### DOM Exposing
So interfacing at the front-end framework level become unscalable, but provides some benefits. If scalability is the issue, then there is really only a single interface to create a scalable product, the DOM.  To solve the problem demonstrated above, our technology reads the DOM and automatically creates invokables.  Take for example our demo application: 

![[in.vestor.app_dash.png]]
*in.vestor.app is an open-source GitHub project for displaying crypto information which we use as a live demo for our technology.  Find the project [here](https://github.com/onur-celik/invester)*

By crawling across the DOM and selecting essential elements such as *buttons, inputs, flagged divs, and more*, our framework is able to circumnavigate the integration issue presented by Manual Function Exposing.  In the above in.vestor.app, buttons such as "Add Widget", "Reset Dashboard", and others are automatically turned into invokables.  This makes our framework completely front-end agnostic so whether a project uses React, Angular, Vue, or something else, integration would work natively.

#### Fusion Approach
Both approaches have unique benefits, so why constrain ourselves to a single way to create them?  In Layer we use a fusion approach that combines DOM invokables with manual invokables.  Although this may seem a bit crazy since some of these invokables are "on-the-fly" (DOM Invokables) meaning they are created at runtime, and others are declared in advance explicitly in code.  To put it all together with a nice bow on top, we will soon provide our customers with a front-end experience that allows developers to declare invokables and create paths without writing code (well at least not much code). See our prototype [here](https://drive.google.com/file/d/1h81P-_zyifdLFnWBN6ZFSpHBETi4z0Bu/view?usp=sharing) (sorry for the low-quality video).

### Planning & Executing Tasks
**omitted**
### Steering
**omitted**
### Other
**omitted**
