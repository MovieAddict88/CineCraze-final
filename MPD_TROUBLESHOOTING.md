# MPD Playback Troubleshooting Guide

## Issue: MPD Shows Duration But No Video/Audio

### 🔍 Common Causes & Solutions

#### 1. **DRM License Issues** (Most Common)
**Symptoms:** Duration shows, no playback, console shows DRM errors

**Solutions:**
- ✅ **Check DRM Configuration**: Ensure `mpdDrm` field has correct license server URL
- ✅ **Verify License Server**: Test if license server is accessible
- ✅ **Browser Support**: Ensure browser supports Widevine/PlayReady
- ✅ **HTTPS Required**: DRM only works over HTTPS

**Example DRM Config:**
```json
{
  "mpdDrm": {
    "com.widevine.alpha": {
      "serverURL": "https://license-server.com/widevine",
      "httpRequestHeaders": {
        "authorization": "Bearer your-token"
      }
    }
  }
}
```

#### 2. **CORS Issues**
**Symptoms:** Network errors, manifest loading fails

**Solutions:**
- ✅ **Server CORS Headers**: Ensure MPD server allows cross-origin requests
- ✅ **CDN Configuration**: Configure CDN to add CORS headers
- ✅ **Proxy Server**: Use a proxy if direct access fails

**Required Headers:**
```
Access-Control-Allow-Origin: *
Access-Control-Allow-Headers: Range
Access-Control-Allow-Methods: GET, HEAD
```

#### 3. **Invalid MPD Manifest**
**Symptoms:** Duration shows but tracks are empty

**Solutions:**
- ✅ **Validate XML**: Check if MPD is valid XML
- ✅ **Required Elements**: Ensure `<Period>` and `<AdaptationSet>` exist
- ✅ **Media URLs**: Verify segment URLs are accessible

#### 4. **Browser Compatibility**
**Symptoms:** Works in some browsers, not others

**Solutions:**
- ✅ **EME Support**: Check if browser supports Encrypted Media Extensions
- ✅ **Codec Support**: Verify browser supports video/audio codecs
- ✅ **Update Browser**: Use latest browser version

**Browser Support Check:**
```javascript
// Check EME support
if ('requestMediaKeySystemAccess' in navigator) {
    console.log('EME supported');
} else {
    console.log('EME not supported');
}
```

#### 5. **Network/Firewall Issues**
**Symptoms:** Manifest loads but segments fail

**Solutions:**
- ✅ **Segment URLs**: Test if media segments are accessible
- ✅ **Firewall**: Check corporate/personal firewall settings
- ✅ **VPN**: Test with/without VPN connection

### 🛠️ Debugging Steps

#### Step 1: Check Browser Console
Look for these specific error messages:

```javascript
// Good signs:
"DASH stream initialized successfully"
"Playback started"
"Available video tracks: [...]"

// Bad signs:
"DRM license error"
"No playable tracks found"
"Network error"
"CORS error"
```

#### Step 2: Test MPD Manifest Directly
Open the MPD URL in browser:
```
https://your-server.com/content.mpd
```

Should return XML content like:
```xml
<?xml version="1.0"?>
<MPD xmlns="urn:mpeg:dash:schema:mpd:2011" 
     mediaPresentationDuration="PT1H30M">
  <Period>
    <AdaptationSet contentType="video">
      <Representation bandwidth="1000000">
        <!-- Video segments -->
      </Representation>
    </AdaptationSet>
  </Period>
</MPD>
```

#### Step 3: Use Enhanced Diagnostics
The updated code includes automatic diagnostics:

```javascript
// Automatic diagnosis runs when playing MPD
// Check console for: "MPD Diagnosis Results"
```

#### Step 4: Test Without DRM
If DRM is causing issues, test with non-protected content first:

```json
{
  "Servers": [
    {
      "name": "1080p",
      "url": "https://test-server.com/test-content.mpd"
    }
  ]
  // No mpdDrm field = no DRM
}
```

### 🔧 Implementation Fixes Applied

#### Enhanced Error Handling
- ✅ Specific DRM error codes with user-friendly messages
- ✅ Automatic retry for network errors
- ✅ Timeout detection for failed initialization

#### Better Diagnostics
- ✅ Pre-flight MPD manifest validation
- ✅ Track availability checking
- ✅ Real-time playback monitoring

#### Improved User Feedback
- ✅ Loading status messages
- ✅ Clear error descriptions
- ✅ Progress indicators

### 📱 Device-Specific Issues

#### Mobile Devices
- **iOS Safari**: Limited DRM support, requires specific configuration
- **Android Chrome**: Full Widevine support
- **Mobile Firefox**: Limited EME support

#### Desktop Browsers
- **Chrome**: Full DRM support (recommended)
- **Firefox**: Good DRM support
- **Safari**: Limited Widevine support
- **Edge**: PlayReady + Widevine support

### 🚨 When to Contact Support

Contact technical support if:
- ✅ Non-DRM content works but DRM content fails
- ✅ Same content works in other players
- ✅ Console shows license server errors
- ✅ Issues persist across multiple browsers

### 📊 Testing Tools

#### Online MPD Validators
- DASH Industry Forum Validator
- Bento4 online tools
- MP4Box.js validator

#### Browser Developer Tools
1. **Network Tab**: Check MPD and segment loading
2. **Console Tab**: Look for JavaScript errors
3. **Security Tab**: Verify HTTPS and certificates

### 💡 Best Practices

#### For Content Providers
- ✅ Use HTTPS for all MPD and segment URLs
- ✅ Configure proper CORS headers
- ✅ Test with multiple DRM systems
- ✅ Provide fallback URLs

#### For Developers
- ✅ Implement comprehensive error handling
- ✅ Add diagnostic logging
- ✅ Test across browsers and devices
- ✅ Provide clear user feedback

---

**Note**: The enhanced MPD playback implementation automatically handles many of these issues and provides detailed diagnostic information in the browser console.