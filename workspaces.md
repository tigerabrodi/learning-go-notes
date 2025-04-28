# Go Workspaces Explained

Go workspaces (introduced in Go 1.18) solve a specific development problem: working on multiple related modules at the same time.

## When to Use Workspaces

Use workspaces when you're:

1. **Developing multiple interdependent modules**  
   Example: Working on both an API client library and the service that uses it

2. **Fixing or enhancing a dependency**  
   Example: Found a bug in a library you use and want to fix it locally before submitting a PR

3. **Working in a monorepo with multiple Go modules**  
   Example: A project with separate frontend, backend, and shared modules

4. **Developing a plugin system**  
   Example: Core application and several plugin modules

## How Workspaces Work

A workspace uses a `go.work` file to tell Go "use these local module directories instead of fetching from the internet":

```
go 1.18

use (
    ./myapp
    ./mylib
)
```

## Workspace Workflow

1. Create modules as normal:

   ```bash
   mkdir -p workspace/myapp workspace/mylib
   cd workspace/myapp
   go mod init example.com/myapp
   cd ../mylib
   go mod init example.com/mylib
   ```

2. Initialize the workspace at the root:

   ```bash
   cd ..
   go work init ./myapp ./mylib
   ```

3. Develop in both modules, with changes immediately visible:

   ```go
   // In mylib/lib.go
   package mylib
   func DoThing() string { return "updated thing" }

   // In myapp/main.go
   package main
   import "example.com/mylib"
   func main() { println(mylib.DoThing()) }
   ```

4. Run from the workspace:
   ```bash
   go run example.com/myapp
   ```

## Important Notes

1. **Don't commit go.work files** - They're for local development only
2. **You still need proper `require` directives** - When publishing, modules need correct dependencies in go.mod
3. **Use `go work sync`** - To update go.mod files before committing

## Real-World Example

```
myproject/
├── go.work           # Points to the modules
├── api/              # API module
│   ├── go.mod        # module example.com/api
│   └── api.go
├── server/           # Server module that imports api
│   ├── go.mod        # module example.com/server
│   └── server.go
└── client/           # Client module that imports api
    ├── go.mod        # module example.com/client
    └── client.go
```

Workspaces let you modify the API while immediately testing the changes in both server and client.
