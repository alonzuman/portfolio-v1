---
title: Typescript Config
layout: ../../layouts/Layout.astro
---

A quick setup for absolute imports in a Typescript project (mapping the `src` folder to `~`)
<br/>

```json
{
	// ...
	"compilerOptions": {
		// ...
		"baseUrl": ".",
		"paths": {
			"~/*": ["src/*"]
		}
	}
	//...
}
```

<br/>
This allows easy imports across the repo:
<br/>
<br/>
❌ Before

```javascript
import foo from "../../../foo";
```

<br/>
✅ After

```javascript
import foo from "~/bar/foo";
```
