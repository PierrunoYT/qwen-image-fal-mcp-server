# Qwen Image fal.ai MCP Server

A Model Context Protocol (MCP) server that provides image generation capabilities using Alibaba's Qwen Image model via the fal.ai platform.

## Features

Qwen Image is an advanced text-to-image foundation model that excels at:

- **Complex text rendering** with accurate text layout in generated images
- **Precise image editing** capabilities
- **High-quality image generation** from detailed text prompts
- **Multiple image sizes and aspect ratios** support
- **Advanced guidance controls** for fine-tuning results
- **Safety checking** and content filtering
- **Multiple output formats** (PNG, JPEG)
- **Batch generation** support (up to 4 images per request)
- **LoRA support** for style customization (up to 3 LoRAs)

## Available Tools

### `generate_image`
Generate high-quality images from text prompts using Qwen Image via fal.ai.

**Parameters:**
- `prompt` (required): Text description of the image to generate (supports detailed prompts)
- `image_size` (optional): Image size/aspect ratio - one of: `square_hd`, `square`, `portrait_4_3`, `portrait_16_9`, `landscape_4_3`, `landscape_16_9` (default: `landscape_4_3`)
- `num_inference_steps` (optional): Number of inference steps, higher = better quality but slower (2-250, default: 30)
- `seed` (optional): Random seed for reproducible results (0-2147483647)
- `guidance_scale` (optional): CFG scale - how closely to follow the prompt (0.0-20.0, default: 2.5)
- `sync_mode` (optional): Wait for generation to complete before returning (default: false)
- `num_images` (optional): Number of images to generate (1-4, default: 1)
- `enable_safety_checker` (optional): Enable content safety filtering (default: true)
- `output_format` (optional): Output format - `jpeg` or `png` (default: `png`)
- `negative_prompt` (optional): Specify what should NOT be in the image (default: "")
- `acceleration` (optional): Speed optimization - `none`, `regular`, or `high` (default: `none`)
- `loras` (optional): Array of LoRA weights for style customization (max 3 LoRAs)

## Installation

### Prerequisites

1. **fal.ai API Key**: Get your API key from [fal.ai](https://fal.ai/)
   - Sign up for an account at https://fal.ai/
   - Navigate to your dashboard and generate an API key
   - Keep this key secure as you'll need it for configuration

2. **Node.js**: Ensure you have Node.js installed (version 18 or higher)

### Quick Setup (Recommended)

The easiest way to use this server is through npx, which automatically downloads and runs the latest version:

#### For Claude Desktop App

Add the server to your Claude Desktop configuration file:

**macOS**: `~/Library/Application Support/Claude/claude_desktop_config.json`
**Windows**: `%APPDATA%\Claude\claude_desktop_config.json`

```json
{
  "mcpServers": {
    "qwen-image": {
      "command": "npx",
      "args": [
        "-y",
        "https://github.com/PierrunoYT/qwen-image-fal-mcp-server.git"
      ],
      "env": {
        "FAL_KEY": "your_fal_key_here"
      }
    }
  }
}
```

#### For Kilo Code MCP Settings

Add to your MCP settings file at:
`C:\Users\[username]\AppData\Roaming\Code\User\globalStorage\kilocode.kilo-code\settings\mcp_settings.json`

```json
{
  "mcpServers": {
    "qwen-image": {
      "command": "npx",
      "args": [
        "-y",
        "https://github.com/PierrunoYT/qwen-image-fal-mcp-server.git"
      ],
      "env": {
        "FAL_KEY": "your_fal_key_here"
      },
      "disabled": false,
      "alwaysAllow": []
    }
  }
}
```

### Benefits of npx Configuration

✅ **Universal Access**: Works on any machine with Node.js
✅ **No Local Installation**: npx downloads and runs automatically
✅ **Always Latest Version**: Pulls from GitHub repository
✅ **Cross-Platform**: Windows, macOS, Linux compatible
✅ **Settings Sync**: Works everywhere you use your MCP client

### Manual Installation (Alternative)

If you prefer to install locally:

1. **Clone the repository**
   ```bash
   git clone https://github.com/PierrunoYT/qwen-image-fal-mcp-server.git
   cd qwen-image-fal-mcp-server
   ```

2. **Install dependencies**
   ```bash
   npm install
   ```

3. **Build the server**
   ```bash
   npm run build
   ```

4. **Use absolute path in configuration**
   ```json
   {
     "mcpServers": {
       "qwen-image": {
         "command": "node",
         "args": ["/absolute/path/to/qwen-image-fal-mcp-server/build/index.js"],
         "env": {
           "FAL_KEY": "your_fal_key_here"
         }
       }
     }
   }
   ```

**Helper script to get the absolute path:**
```bash
npm run get-path
```

## Usage Examples

Once configured, you can use the server through your MCP client:

### Basic Image Generation
```
Generate an image of a serene mountain landscape at sunset with a lake reflection
```

### Complex Text Rendering
```
Create a vintage poster with the text "Welcome to AI Art" in ornate lettering, art deco style
```

### Specific Image Size
```
Generate a portrait-oriented image of a futuristic cityscape (landscape_16_9)
```

### High-Quality Generation
```
Generate a photorealistic portrait with 50 inference steps and guidance scale 7.5
```

### Batch Generation
```
Generate 3 variations of a cute robot character
```

### Using LoRAs for Style
```
Generate a cyberpunk street scene using anime style LoRA with weight 0.8
```

### Advanced Parameters
```
Create a detailed fantasy landscape with negative prompt "blurry, low quality" and seed 12345 for reproducibility
```

## API Response Format

The server returns detailed information about generated images:

```
✅ Successfully generated 1 image(s) using Qwen Image:

📝 **Generation Details:**
• Prompt: "a serene mountain landscape at sunset"
• Image Size: landscape_4_3
• Inference Steps: 30
• Guidance Scale: 2.5
• Seed Used: 1234567890
• Output Format: png
• Acceleration: none
• Safety Checker: Enabled
• Generation Time: 3245ms
• Request ID: req_abc123...

🖼️ **Generated Images (1 total, 1 downloaded):**
• Image 1 (1024x768): ./images/qwen_image_mountain_landscape_0_2024-01-15T10-30-45.png (https://v3.fal.media/files/...)

💾 Images have been downloaded to the local 'images' directory.
```

## Development

### Local Testing
```bash
# Test the server directly
echo '{"jsonrpc": "2.0", "id": 1, "method": "tools/list"}' | node build/index.js
```

### Watch Mode
```bash
npm run watch
```

### Run Test Server
```bash
npm run test:server
```

### Health Check
```bash
npm run health-check
```

### Inspector Tool
```bash
npm run inspector
```

## Environment Variables

### Required
- `FAL_KEY`: Your fal.ai API key (required for image generation)

### Optional
- `NODE_ENV`: Environment mode (`development`, `production`, `test`) - default: `production`
- `LOG_LEVEL`: Logging level (`error`, `warn`, `info`, `debug`) - default: `info`
- `MAX_CONCURRENT_REQUESTS`: Maximum concurrent requests (1-10) - default: 3
- `REQUEST_TIMEOUT`: Request timeout in milliseconds (30000-600000) - default: 300000

## Troubleshooting

### Common Issues

1. **"FAL_KEY environment variable is required"**
   - The server will continue running and show this helpful error message
   - Ensure your fal.ai API key is properly set in the MCP configuration
   - Verify the key is valid and has sufficient credits
   - **Note**: The server no longer crashes when the API key is missing

2. **"Server not showing up in Claude"**
   - If using npx configuration, ensure you have Node.js installed (v18+)
   - For manual installation, check that the absolute path is correct
   - Restart Claude Desktop after configuration changes
   - Verify the JSON configuration syntax is valid

3. **"Generation failed"**
   - Check your fal.ai account has sufficient credits
   - Verify your API key has the necessary permissions
   - Try with a simpler prompt to test connectivity
   - Check if the image size and parameters are valid

4. **"npx command not found"**
   - Ensure Node.js is properly installed
   - Try running `node --version` and `npm --version` to verify installation

### Server Stability Improvements

✅ **Robust Error Handling**: Server continues running even without API key
✅ **Graceful Shutdown**: Proper handling of SIGINT and SIGTERM signals
✅ **User-Friendly Messages**: Clear error messages with setup instructions
✅ **No More Crashes**: Eliminated `process.exit()` calls that caused connection drops
✅ **Local Image Storage**: Downloads generated images for offline access

### Debug Logging

The server outputs debug information to stderr, which can help diagnose issues:
- Generation progress updates
- Error messages with helpful instructions
- API call details
- Graceful shutdown notifications

### Performance Tips

- Use `acceleration: "regular"` for balanced speed/quality
- Use `acceleration: "high"` for images without text
- Lower `num_inference_steps` for faster generation (try 20-25)
- Use `sync_mode: true` for immediate results (increases latency)
- Cache results using the `seed` parameter for reproducibility

## Pricing

Image generation costs are determined by fal.ai's pricing structure. Check [fal.ai Pricing](https://fal.ai/pricing) for current rates.

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests if applicable
5. Submit a pull request

## Support

For issues related to:
- **This MCP server**: Open an issue in this repository
- **fal.ai API**: Contact fal.ai support
- **Qwen Image model**: Refer to fal.ai documentation

## Changelog

### v1.0.0 (Latest)
- **🎨 Qwen Image Integration**: Full implementation of Qwen Image model via fal.ai
- **🔧 Fixed connection drops**: Removed `process.exit()` calls that caused server crashes
- **🛡️ Improved error handling**: Server continues running even without API key
- **🌍 Added portability**: npx configuration works on any machine
- **📦 Enhanced stability**: Graceful shutdown handlers and null safety checks
- **💬 Better user experience**: Clear error messages with setup instructions
- **🔄 Auto-updating**: npx pulls latest version from GitHub automatically
- **📁 Local image storage**: Downloads generated images to local directory
- **🎯 Advanced parameters**: Support for all Qwen Image parameters including LoRAs
- **⚡ Performance optimization**: Configurable acceleration levels
- **🛡️ Safety features**: Built-in content safety checking

### v0.1.1
- Initial fal.ai integration
- Basic image generation functionality

### v0.1.0
- Initial release with placeholder implementation

## Additional Resources

- [Qwen Image Model Playground](https://fal.ai/models/fal-ai/qwen-image)
- [fal.ai API Documentation](https://docs.fal.ai)
- [Model Context Protocol Specification](https://modelcontextprotocol.io/)
- [MCP SDK Documentation](https://github.com/modelcontextprotocol/typescript-sdk)
