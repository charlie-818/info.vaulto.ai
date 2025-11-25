# DNS Troubleshooting Guide for info.vaulto.ai

## Current DNS Configuration

### IPv4 Records (A Records)
- `52.52.192.191`
- `13.52.188.95`

### IPv6 Records (AAAA Records)
- `2600:1f1c:446:4900::258`
- `2600:1f1c:446:4900::259`

### DNS Status
- ✅ DNS records are properly configured
- ✅ Both IPv4 and IPv6 records are present
- ✅ Records resolve correctly from Google DNS (8.8.8.8) and Cloudflare DNS (1.1.1.1)

## Mobile DNS Issues - Common Causes

### 1. DNS Propagation Delay
Mobile carriers often cache DNS records aggressively. If the domain was recently configured:
- **Wait time**: 24-48 hours for full propagation
- **TTL**: Check current TTL settings in Netlify DNS dashboard

### 2. Mobile Carrier DNS Caching
Mobile networks use their own DNS servers which may cache old records:
- **Solution**: Clear DNS cache on mobile device
- **Android**: Settings → Network → Advanced → Private DNS → Off (then back to Auto)
- **iOS**: Settings → General → Reset → Reset Network Settings

### 3. IPv6 vs IPv4 Issues
Some mobile carriers prefer IPv6, others IPv4:
- ✅ Both record types are configured
- If issues persist, verify mobile carrier supports IPv6

### 4. Netlify Domain Configuration
Ensure in Netlify Dashboard:
- Domain is added and verified
- SSL certificate is active
- DNS records match Netlify's requirements

## Verification Commands

### Check DNS Resolution
```bash
# Standard DNS lookup
dig info.vaulto.ai +short

# Check IPv4 records
dig info.vaulto.ai A +short

# Check IPv6 records
dig info.vaulto.ai AAAA +short

# Check from Google DNS
dig @8.8.8.8 info.vaulto.ai +short

# Check from Cloudflare DNS
dig @1.1.1.1 info.vaulto.ai +short
```

### Online DNS Checkers
- https://dnschecker.org/#A/info.vaulto.ai
- https://www.whatsmydns.net/#A/info.vaulto.ai
- https://mxtoolbox.com/DNSLookup.aspx

## Troubleshooting Steps

### Step 1: Verify DNS Records
Run the verification commands above to ensure DNS records are correct.

### Step 2: Check Netlify Dashboard
1. Go to Netlify Dashboard → Site Settings → Domain Management
2. Verify `info.vaulto.ai` is listed and verified
3. Check SSL certificate status (should be "Active")
4. Verify DNS records match what's shown in DNS verification

### Step 3: Test from Mobile Device
1. **Clear DNS cache** on mobile device (see instructions above)
2. **Try different networks**:
   - WiFi (home/office)
   - Mobile data (carrier network)
   - Different mobile carrier if possible
3. **Try different browsers** on mobile:
   - Chrome
   - Safari
   - Firefox

### Step 4: Check Mobile Carrier DNS
Some carriers have DNS issues. Try:
- **Change DNS on mobile**: Use Google DNS (8.8.8.8) or Cloudflare DNS (1.1.1.1)
- **Android**: Settings → Network → Advanced → Private DNS → Private DNS provider hostname → `dns.google` or `1dot1dot1dot1.cloudflare-dns.com`
- **iOS**: Requires VPN or network-level DNS change

### Step 5: Verify Redirect Works
Once DNS resolves, test the redirect:
- Should redirect to `https://vaulto.ai`
- Should use 301 permanent redirect
- Should work on both HTTP and HTTPS

## Netlify Configuration Files

### netlify.toml
```toml
[[redirects]]
  from = "/*"
  to = "https://vaulto.ai"
  status = 301
  force = true
```

### _redirects
```
/*    https://vaulto.ai    301
```

Both files ensure redirect works even if DNS has temporary issues.

## Expected Behavior

### Desktop (Working)
- ✅ DNS resolves correctly
- ✅ Redirects to vaulto.ai
- ✅ Works on both HTTP and HTTPS

### Mobile (Should Work)
- DNS should resolve (may take 24-48 hours after initial setup)
- Redirect should work once DNS resolves
- If DNS doesn't resolve, check mobile carrier DNS settings

## Contact Netlify Support

If DNS issues persist after 48 hours:
1. Check Netlify Status: https://www.netlifystatus.com/
2. Contact Netlify Support with:
   - Domain: info.vaulto.ai
   - DNS records (from verification commands)
   - Mobile carrier information
   - Screenshots of error messages

## Last Verified
- Date: 2025-11-25
- DNS Status: ✅ Resolving correctly
- IPv4: ✅ Present
- IPv6: ✅ Present
- Redirect: ✅ Configured

