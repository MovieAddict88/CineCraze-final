# CineCraze - Enhanced MPD Playback with DRM Support

## Overview

This enhanced version of CineCraze includes comprehensive support for DASH MPD playback with various DRM systems. The implementation provides a robust, user-friendly streaming experience with advanced error handling and quality control.

## 🎯 Key Features

### MPD & DRM Support
- **Widevine DRM** (`com.widevine.alpha`)
- **PlayReady DRM** (`com.microsoft.playready`)  
- **ClearKey DRM** (`org.w3.clearkey`)
- **Multi-DRM** support for failover scenarios
- **Unprotected MPD** playback

### Enhanced Player Features
- **Adaptive Quality Selection** - Automatic and manual quality switching
- **Real-time DRM Status** - Visual indicators for protection status
- **Enhanced Error Handling** - Specific error messages and retry logic
- **Loading Feedback** - Animated loading states during license acquisition
- **Buffer Optimization** - Tuned for smooth playback experience

## 🔧 Configuration

### Basic MPD Setup (No DRM)
```json
{
  "Title": "Sample Movie",
  "Servers": [
    {
      "name": "1080p",
      "url": "https://example.com/movie.mpd"
    }
  ]
}
```

### Widevine DRM Configuration
```json
{
  "Title": "Protected Movie",
  "Servers": [
    {
      "name": "1080p",
      "url": "https://example.com/protected-movie.mpd"
    }
  ],
  "mpdDrm": {
    "widevine": {
      "licenseUrl": "https://drm.example.com/license",
      "headers": {
        "X-DRM-AuthToken": "your-auth-token",
        "Content-Type": "application/json"
      },
      "withCredentials": false
    }
  }
}
```

### PlayReady DRM Configuration
```json
{
  "Title": "Enterprise Content",
  "Servers": [
    {
      "name": "720p",
      "url": "https://example.com/enterprise.mpd"
    }
  ],
  "mpdDrm": {
    "playready": {
      "licenseUrl": "https://drm.example.com/playready",
      "headers": {
        "Authorization": "Bearer your-token"
      }
    }
  }
}
```

### ClearKey DRM Configuration
```json
{
  "Title": "ClearKey Content",
  "Servers": [
    {
      "name": "1080p",
      "url": "https://example.com/clearkey.mpd"
    }
  ],
  "mpdDrm": {
    "clearkey": {
      "keys": {
        "key-id-1": "key-value-1",
        "key-id-2": "key-value-2"
      }
    }
  }
}
```

### Multi-DRM Configuration
```json
{
  "Title": "Multi-DRM Content",
  "Servers": [
    {
      "name": "4K",
      "url": "https://example.com/4k-content.mpd"
    }
  ],
  "mpdDrm": {
    "widevine": {
      "licenseUrl": "https://drm.example.com/widevine",
      "headers": { "Authorization": "Bearer token" }
    },
    "playready": {
      "licenseUrl": "https://drm.example.com/playready", 
      "headers": { "Authorization": "Bearer token" }
    }
  }
}
```

## 🚀 Enhanced Features

### Quality Selection UI
- Automatic quality dropdown appears for DASH content
- Manual quality override capability
- Real-time quality change notifications
- Optimized for different screen sizes

### DRM Status Indicators
- 🔒 **Protected Content** - Shows active DRM system
- 🔓 **Unprotected Content** - Shows for non-DRM MPD files
- ❌ **DRM Error** - Shows when license acquisition fails

### Error Handling
- **Network Errors**: Automatic retry with progressive delays
- **DRM Errors**: Specific error messages for license issues
- **Capability Errors**: Device compatibility warnings
- **Manifest Errors**: Intelligent fallback to alternative servers

### Buffer Management
- Optimized buffer settings for smooth playback
- Adaptive buffering based on content duration
- Gap jumping for seamless experience
- Configurable retry attempts and timeouts

## 🎮 Usage Examples

### Loading Protected Content
```javascript
// The system automatically detects MPD URLs and DRM configuration
const content = {
  Title: "Protected Movie",
  Servers: [{ name: "1080p", url: "https://example.com/movie.mpd" }],
  mpdDrm: {
    widevine: {
      licenseUrl: "https://drm.example.com/license",
      headers: { "Authorization": "Bearer token123" }
    }
  }
};

// Simply call openViewer - DRM setup is automatic
openViewer(content);
```

### Handling Different Quality Levels
```javascript
// Multiple quality servers are automatically detected
const content = {
  Title: "Multi-Quality Content",
  Servers: [
    { name: "4K", url: "https://example.com/4k.mpd" },
    { name: "1080p", url: "https://example.com/1080p.mpd" },
    { name: "720p", url: "https://example.com/720p.mpd" }
  ]
};
```

## 🔍 Troubleshooting

### Common Issues

**License Acquisition Failed**
- Check license server URL and authentication headers
- Verify device supports the DRM system
- Ensure proper CORS configuration on license server

**Quality Selection Not Appearing**
- Confirm MPD manifest contains multiple representations
- Check browser compatibility with DASH.js
- Verify network connectivity to manifest URL

**Playback Stuttering**
- Adjust buffer settings in `configureDashPlayer()` function
- Check available bandwidth and reduce quality if needed
- Verify CDN performance and geographic distribution

### Browser Compatibility
- **Chrome/Edge**: Full Widevine and PlayReady support
- **Firefox**: Widevine support, limited PlayReady
- **Safari**: Limited DRM support, best with unprotected content
- **Mobile**: Device-dependent DRM capabilities

## 🛠️ Advanced Configuration

### Custom Buffer Settings
```javascript
// Modify in configureDashPlayer() function
player.updateSettings({
  'streaming': {
    'buffer': {
      'bufferTimeAtTopQuality': 30,        // Seconds
      'initialBufferLevel': 20,            // Seconds
      'stableBufferTime': 12              // Seconds
    }
  }
});
```

### Custom Error Handling
```javascript
// Extend handleDashPlayerError() function for custom logic
function handleDashPlayerError(e, url, drmConfig) {
  // Your custom error handling logic here
  // Call showErrorMessage() for user feedback
  // Implement custom retry strategies
}
```

## 📊 Performance Optimizations

1. **Lazy Loading**: Quality selector loads only when needed
2. **Memory Management**: Proper cleanup of DASH players and timers
3. **Network Optimization**: Intelligent retry logic with exponential backoff
4. **UI Responsiveness**: Non-blocking DRM initialization
5. **Error Recovery**: Automatic fallback to alternative servers

## 🔐 Security Considerations

- License URLs should use HTTPS
- Implement proper authentication tokens
- Use withCredentials carefully based on CORS requirements
- Regular rotation of DRM keys and tokens
- Monitor for unauthorized access attempts

## 📱 Mobile Optimizations

- Touch-friendly quality selector
- Optimized buffer sizes for mobile networks
- Reduced memory footprint
- Battery-aware streaming settings
- Adaptive quality based on connection type

---

*For additional support or questions about MPD and DRM implementation, please refer to the comprehensive documentation in the source code comments.*