# React Okta Integration

A React application demonstrating Okta authentication integration using the [Okta React Library](https://github.com/okta/okta-react) and [React Router](https://github.com/ReactTraining/react-router). This application implements secure user authentication through the [PKCE Flow](https://developer.okta.com/docs/concepts/oauth-openid/#authorization-code-flow-with-pkce), where users are redirected to the Okta-hosted login page and returned with ID and access tokens.

## Prerequisites

Before you begin, you’ll need an Okta Integrator Free Plan account. To get one, sign up for an [Integrator account](https://developer.okta.com/login). Once you have an account, sign in to your [Integrator account](https://developer.okta.com/login). Next, in the Admin Console:

1. Go to **Applications > Applications**
2. Click **Create App Integration**
3. Select **OIDC - OpenID Connect** as the sign-in method
4. Select **Single-Page Application** as the application type, then click **Next**
5. Enter an app integration name
6. In the **Grant type** section, ensure both **Authorization Code** and **Refresh Token** are selected
7. Configure the redirect URIs:
  - **Sign-in redirect URIs:** `http://localhost:3000/callback`
  - **Sign-out redirect URIs:** `http://localhost:3000`
8. In the **Controlled access** section, select the appropriate access level
9. Click **Save**

## Configure Okta resources

**Enable Refresh Tokens**

Sign into your [Okta Developer Edition account](https://developer.okta.com/login/) to add a required setting to your React Okta app to avoid third-party cookies. Navigate to **Applications** > **Applications** and select your application to edit. Find the **General Settings** and press **Edit**. Enable **Refresh Token** in the **Grant type** section. **Save** your changes.

**Verify Authorization Server**

When using a custom authorization server, you need to set up authorization policies. Complete these additional steps:

1. In the Admin Console, go to **Security > API > Authorization Servers**
2. Select your custom authorization server (`default`)
3. On the **Access Policies** tab, ensure you have at least one policy:
    - If no policies exist, click **Add New Access Policy**
    - Give it a name like “Default Policy”
    - Set **Assign to** to “All clients”
    - Click **Create Policy**
4. For your policy, ensure you have at least one rule:
    - Click **Add Rule** if no rules exist
    - Give it a name like “Default Rule”
    - Set **Grant type** is to “Authorization Code”
    - Set **User** is to “Any user assigned the app”
    - Set **Scopes requested** to “Any scopes”
    - Click **Create Rule**

For more details, see the [Custom Authorization Server documentation](https://developer.okta.com/docs/concepts/auth-servers/#custom-authorization-server).

## Get the Code

```bash
git clone https://github.com/harishkaparwan/react-okta-integration.git
cd react-okta-integration
```

## Configuration

Update your `.okta.env` file with the values from your Okta application's configuration:

```text
ISSUER=https://your-okta-domain.okta.com/oauth2/default
CLIENT_ID=your_client_id_here
```

**Important:** Replace the placeholder values with your actual Okta configuration:
- `ISSUER`: Your Okta domain authorization server URL
- `CLIENT_ID`: Your application's Client ID from the Okta Admin Console

### Where are my new app's credentials?

Creating an OIDC Single-Page App manually in the Admin Console configures your Okta Org with the application settings. You may also need to configure trusted origins for `http://localhost:3000` in **Security > API > Trusted Origins**.

After creating the app, you can find the configuration details on the app's **General** tab:
- **Client ID**: Found in the **Client Credentials** section
- **Issuer**: Found in the **Issuer URI** field for the authorization server that appears by selecting **Security > API** from the navigation pane.

## Prerequisites

- Node.js 18+ (recommended: use Node.js 20+)
- npm or yarn package manager
- An Okta Developer account

## Run the Application

1. **Install dependencies:**
   ```bash
   npm install
   ```

2. **Configure environment variables:**
   Update the `.okta.env` file with your Okta application credentials.

3. **Start the development server:**
   ```bash
   npm start
   ```

4. **Open your browser:**
   Navigate to `http://localhost:3000` in your browser.

If you see a home page that prompts you to login, then things are working! Clicking the **Log in** button will redirect you to the Okta hosted sign-in page.

You can sign in with the same account that you created when signing up for your Developer Org, or you can use a known username and password from your Okta Directory.

> **Note:** If you are currently using your Developer Console, you already have a Single Sign-On (SSO) session for your Org. You will be automatically logged into your application as the same user that is using the Developer Console. You may want to use an incognito tab to test the flow from a blank slate.

## Project Structure

```
react-okta-integration/
├── public/
│   ├── index.html
│   └── manifest.json
├── src/
│   ├── components/
│   │   ├── Loading.jsx
│   │   ├── Routes.jsx
│   │   └── SecureRoute.jsx
│   ├── pages/
│   │   ├── Home.jsx
│   │   ├── Messages.jsx
│   │   ├── Profile.jsx
│   │   └── __tests__/
│   ├── App.jsx
│   ├── App.css
│   ├── Navbar.jsx
│   ├── config.js
│   ├── index.jsx
│   └── index.css
├── .okta.env
├── package.json
├── vite.config.js
└── README.md
```

## Technology Stack

- **React 18** - Frontend framework
- **Vite** - Build tool and development server
- **React Router v7** - Client-side routing
- **Okta React SDK** - Authentication integration
- **Semantic UI React** - UI components
- **Vitest** - Testing framework

## Troubleshooting

### Common Issues

1. **Application type error**: "Clients with 'application_type' of 'service' are not allowed to access the 'authorize' endpoint"
   - Ensure your Okta application is configured as **Single-Page Application**, not Service or Web Application
   - Check that **Authorization Code** and **Refresh Token** grant types are enabled

2. **Node.js version compatibility**
   - This project requires Node.js 18+ (Vite 5 requirement)
   - Use `nvm use 20` if you have multiple Node.js versions

3. **Environment variables not loading**
   - Verify `.okta.env` contains actual values, not placeholder variables
   - Ensure ISSUER and CLIENT_ID are properly set

4. **Port conflicts**
   - The app runs on port 3000 by default
   - Ensure your Okta redirect URIs match the port you're using

### Testing the Application

Run the test suite:
```bash
npm test
```

## Integrating with Resource Servers

If you were able to successfully login, you can integrate with backend resource servers. Example resource server implementations:

* [Node/Express Resource Server Example](https://github.com/okta/samples-nodejs-express-4/tree/master/resource-server)
* [Java/Spring MVC Resource Server Example](https://github.com/okta/samples-java-spring/tree/master/resource-server)
* [ASP.NET](https://github.com/okta/samples-aspnet/tree/master/resource-server) and [ASP.NET Core](https://github.com/okta/samples-aspnetcore/tree/master/samples-aspnetcore-3x/resource-server) Resource Server Examples

## Contributing

1. Fork this repository
2. Create a feature branch: `git checkout -b feature/your-feature`
3. Commit your changes: `git commit -am 'Add some feature'`
4. Push to the branch: `git push origin feature/your-feature`
5. Submit a pull request

## License

This project is licensed under the Apache License 2.0 - see the original [Okta samples](https://github.com/okta-samples) for details.

Once you have the resource server running (it will run on port 8000) you can visit the `/messages` page within the React application to see the authentication flow. The React application will use its stored access token to authenticate itself with the resource server, you will see this as the `Authorization: Bearer <access_token>` header on the request if you inspect the network traffic in your browser.

[Okta React Library]: https://github.com/okta/okta-react
[OIDC SPA Setup Instructions]: https://developer.okta.com/docs/guides/sign-into-spa/react/before-you-begin
[PKCE Flow]: https://developer.okta.com/docs/guides/implement-auth-code-pkce
[Okta Sign In Widget]: https://github.com/okta/okta-signin-widget
