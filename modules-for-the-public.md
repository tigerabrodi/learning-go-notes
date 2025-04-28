1. Create your module:

   ```bash
   mkdir mymodule
   cd mymodule
   go mod init github.com/yourusername/mymodule
   ```

2. Create your code files and develop your package

3. Test your code:

   ```bash
   go test ./...
   ```

4. Initialize Git and commit your code:

   ```bash
   git init
   git add .
   git commit -m "Initial commit"
   ```

5. Create a repo on GitHub/GitLab/etc. with the same name as your module

6. Push to GitHub:

   ```bash
   git remote add origin https://github.com/yourusername/mymodule.git
   git push -u origin main
   ```

7. Tag a version:
   ```bash
   git tag v0.1.0
   git push origin v0.1.0
   ```

That's it! Your module is now published and can be installed with:

```bash
go get github.com/yourusername/mymodule
```

The Go module proxy will automatically discover and index your module after someone first requests it. No separate "publish" command or registry upload is needed.
