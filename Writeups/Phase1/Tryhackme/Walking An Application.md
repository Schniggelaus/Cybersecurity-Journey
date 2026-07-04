## Walking An Application – Browser-based Recon

## What to Look For
 
| Area | What to Check | Why It Matters |
|------|--------------|----------------|
| **HTML Source** | Comments, hidden fields, framework hints, version numbers | Developers leave debug info, credentials, or internal notes in comments |
| **CSS** | Elements hidden via `display:none` or `visibility:hidden` | UI elements are only visually hidden – still present in DOM, fully accessible |
| **JavaScript** | Paywalls, feature locks, client-side validation | If enforcement only happens in JS, it can be bypassed directly in the browser |
| **Debugger / Breakpoints** | Pause JS execution, inspect and modify variables at runtime | Allows bypassing functions like login checks or payment walls before they execute |
| **Network Tab** | Outgoing requests, API endpoints, parameters being sent | Reveals hidden API calls, authentication tokens, and backend structure |
| **Framework Info** | Framework name and version in source or response headers | Outdated frameworks have known CVEs that are publicly searchable |
| **Directory Access** | Manually browse to paths like `assets` to see ful Web directory | Web servers sometimes expose directories in imported stylesheets or scripts without linking to them – directly accessible if not restricted |
---
 
## Key Takeaway
 
> Everything the browser renders, the user can see and manipulate.
> Security must never rely solely on client-side enforcement – every validation needs to be repeated server-side.
 
