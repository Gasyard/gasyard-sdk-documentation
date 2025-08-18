# Gasyard SDK API Documentation

A modern, interactive API documentation interface for the Gasyard SDK, inspired by platforms like Relay. This documentation provides a clean, professional interface for exploring and testing your API endpoints.

## Features

### 🎨 Modern Design

- Clean, professional interface similar to Relay's documentation
- Responsive design that works on desktop and mobile devices
- Custom styling with modern typography and color schemes

### 📱 Interactive Navigation

- Sidebar navigation with organized endpoint categories
- HTTP method badges (GET, POST, PUT, DELETE) with color coding
- Smooth scrolling to specific endpoints
- Mobile-friendly collapsible sidebar

### 🔧 Enhanced Functionality

- Interactive API testing directly in the browser
- Request/response examples
- Parameter validation
- Real-time API calls with proper error handling

### 📋 Organized Structure

- Grouped endpoints by SDK version (SDK and SDK V2)
- Clear endpoint descriptions and parameters
- Visual hierarchy for better readability

## Getting Started

### Prerequisites

- A web server to serve the files (local or hosted)
- Your `swagger.yaml` file in the same directory

### Running Locally

1. **Using Python (Simple HTTP Server)**

   ```bash
   # Python 3
   python -m http.server 8000

   # Python 2
   python -m SimpleHTTPServer 8000
   ```

2. **Using Node.js (http-server)**

   ```bash
   npx http-server -p 8000
   ```

3. **Using PHP**

   ```bash
   php -S localhost:8000
   ```

4. **Using Live Server (VS Code Extension)**
   - Install the "Live Server" extension
   - Right-click on `index.html` and select "Open with Live Server"

### Accessing the Documentation

Open your browser and navigate to:

- `http://localhost:8000` (if using the above methods)
- Or the URL where you've hosted the files

## File Structure

```
swagger-ui/
├── index.html          # Main documentation interface
├── swagger.yaml        # Your OpenAPI specification
└── README.md          # This file
```

## Customization

### Styling

The interface uses custom CSS that can be modified in the `<style>` section of `index.html`. Key customization areas:

- **Colors**: Modify the CSS custom properties for brand colors
- **Typography**: Change font families and sizes
- **Layout**: Adjust sidebar width, spacing, and responsive breakpoints

### Navigation

To add or modify navigation items, edit the sidebar HTML structure in `index.html`:

```html
<div class="nav-section">
  <div class="nav-section-title">Your Section</div>
  <a
    href="#endpoint-id"
    class="nav-item method"
    onclick="scrollToEndpoint('endpoint-id')"
  >
    <span class="method-badge method-get">GET</span>
    Endpoint Name
  </a>
</div>
```

### Swagger UI Configuration

Modify the Swagger UI initialization in the JavaScript section:

```javascript
SwaggerUIBundle({
  url: "swagger.yaml",
  dom_id: "#swagger-ui",
  deepLinking: true,
  presets: [SwaggerUIBundle.presets.apis, SwaggerUIStandalonePreset],
  // Add more configuration options here
});
```

## API Endpoints

### SDK Endpoints

- **GET /api/sdk/config** - Get configuration data for bridge tokens
- **GET /api/sdk/quote** - Get quote data for bridge tokens
- **GET /api/sdk/bridge** - Initiate bridge transaction
- **GET /api/sdk/balance** - Get portfolio balance for user address
- **GET /api/sdk/process-payment-quote** - Get quote for merchant payment
- **GET /api/sdk/status/{txid}** - Get order status

### SDK V2 Endpoints

- **GET /api/sdk/v2/quote** - Enhanced quote endpoint with additional features
- **GET /api/sdk/v2/bridge** - Enhanced bridge endpoint

## Browser Compatibility

- Chrome 60+
- Firefox 55+
- Safari 12+
- Edge 79+

## Deployment

### Static Hosting

You can deploy this documentation to any static hosting service:

- **GitHub Pages**: Push to a repository and enable GitHub Pages
- **Netlify**: Drag and drop the folder to Netlify
- **Vercel**: Connect your repository to Vercel
- **AWS S3**: Upload files to an S3 bucket with static website hosting

### Custom Domain

To use a custom domain:

1. Configure your domain's DNS settings
2. Update the `servers` section in your `swagger.yaml` file
3. Deploy to your hosting provider

## Contributing

To contribute to this documentation:

1. Fork the repository
2. Make your changes
3. Test locally
4. Submit a pull request

## Support

For issues or questions:

- Check the browser console for JavaScript errors
- Verify your `swagger.yaml` file is valid
- Ensure your web server is properly configured

## License

This documentation interface is open source and available under the MIT License.
