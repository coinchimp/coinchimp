## Creating an Alias in Your `.npmrc` File for Forked Packages

When working with npm packages, there may be times when you need to use a forked version of a package without modifying all your `package.json` files across different projects. Fortunately, npm provides a way to create aliases for packages, allowing you to point the original package name to the forked package using the `.npmrc` file.

Here's a step-by-step guide on how to achieve this:

### 1. Create a `.npmrc` File with Alias

First, you need to create or edit your `.npmrc` file to include the alias. This file should be located at the root of your project or in your home directory.

#### Example Configuration

Let's assume:
- **Original Package**: `@original/package`
- **Forked Package**: `@coinchimp/package`
- **Registry URL**: `https://npm.pkg.github.com/`
- **Auth Token**: `ghp_AAAAAAAAAAAAAAAAAAAA`

Add the following lines to your `.npmrc` file:

```plaintext
@coinchimp:registry=https://npm.pkg.github.com/
//npm.pkg.github.com/:_authToken=ghp_AAAAAAAAAAAAAAAAAAAA
@original/package: npm:@coinchimp/package
```

### 2. Use the Alias in Your Projects

With this configuration in place, any time you install `@original/package`, npm will actually install `@coinchimp/package`.

#### Example `.npmrc` File

Here's how your complete `.npmrc` file might look:

```plaintext
@coinchimp:registry=https://npm.pkg.github.com/
//npm.pkg.github.com/:_authToken=ghp_AAAAAAAAAAAAAAAAAAAA
@original/package: npm:@coinchimp/package
```

### 3. Testing the Alias

To verify that the alias is working correctly, follow these steps:

1. **Create a New Project**

   Create a new project or navigate to an existing one where you want to use the forked package.

   ```bash
   mkdir test-project
   cd test-project
   npm init -y
   ```

2. **Install the Original Package**

   Install the original package using npm. Due to the alias configuration, npm will resolve this to your forked package.

   ```bash
   npm install @original/package
   ```

   If everything is set up correctly, npm will install `@coinchimp/package` instead of `@original/package`.

### Conclusion

By setting up an alias in your `.npmrc` file, you can seamlessly switch to a forked version of a package without having to edit `package.json` files in multiple projects. This approach simplifies dependency management and provides flexibility for using different versions or forks of packages.

Feel free to reach out if you have any questions or need further assistance with this setup!