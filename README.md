# Mihomo Configuration

A comprehensive and optimized configuration for [Mihomo](https://github.com/MetaCubeX/mihomo) (a Clash Meta core). This setup is designed for performance, flexibility, and robust rule-based traffic routing.

---

## ✨ Key Features

* **TUN Mode**: System-wide traffic proxying on macOS, Linux, and Windows.
* **Rule-Based Routing**: Intelligently directs traffic based on domain, IP, or process name.
* **DNS Fake-IP**: Enhances performance and prevents DNS leaks by resolving domains locally.
* **Dynamic Proxy Groups**:
    * `url-test` for automatic fastest node selection.
    * `load-balance` for distributing traffic across multiple nodes.
    * `select` for manual proxy selection.
* **Application-Specific Routing**: Dedicated rules and groups for apps like WhatsApp and ChatGPT.
* **Ad & Malware Blocking**: Blocks ads, malware, phishing, and crypto-miners using community-maintained rule sets.
* **Regional Traffic Handling**: Routes traffic for specific regions (e.g., Iran) directly to improve latency.
* **Automatic Updates**: Rule sets and GeoIP databases are updated automatically.

---

## 🔧 Configuration Highlights

This configuration is structured to prioritize performance and security.

### Approach & Intuition

The core idea is to process traffic through a logical set of rules:

1.  **Block Malicious Content**: The first rules `REJECT` traffic associated with ads, malware, and phishing to enhance security and privacy.
2.  **Direct Local Traffic**: Iranian domains and IPs are routed `DIRECT`ly to ensure low latency for local services.
3.  **Route Specific Apps**: Traffic from applications like WhatsApp and ChatGPT is handled by dedicated `select` groups, giving you manual control over their connection.
4.  **Default to Smart Proxy**: All other traffic (`MATCH`) is routed through the `Smart VPN` group, which can be configured to use the fastest node (`Auto`), balance the load (`Load Balancer`), or use a specific proxy.

### DNS & TUN Mode

* **DNS (`fake-ip`)**: The configuration uses `fake-ip` mode. Mihomo's DNS server intercepts DNS queries and returns a fake IP from the `198.18.0.1/16` range. This allows the TUN device to capture the traffic and decide how to route it based on the original domain name, which is much more reliable than routing by IP alone.
* **TUN (`gvisor`)**: TUN mode creates a virtual network interface to capture all system traffic. `gvisor` is used as the network stack, offering a good balance between performance and compatibility. `dns-hijack` ensures that all DNS requests are redirected to Mihomo's internal DNS server.

---

## 🚀 How to Use

1.  **Prerequisites**: Make sure you have [Mihomo](https://github.com/MetaCubeX/mihomo/releases) installed.

2.  **Download**: Save the configuration file as `config.yaml` in your Mihomo configuration directory (e.g., `~/.config/mihomo/`).

3.  **Add Your Proxies**: This configuration is a template. You **must** add your own proxy servers. You have two options:
    * **Option A (Recommended): Update Proxy Provider**
        Replace the `url` in the `proxy-providers` section with your own subscription link.

        ```yaml
        proxy-providers:
          bpb:
            type: http
            # Change this URL to your own subscription link
            url: "YOUR_SUBSCRIPTION_URL_HERE"
            path: ./providers/bpb.yaml
            health-check:
              # ...
        ```

    * **Option B: Add Proxies Manually**
        Add your proxy definitions to the `proxies` section and update the `proxy-groups` to include them.

        ```yaml
        proxies:
          # Replace with your actual proxy configurations
          - name: "My SS Proxy"
            type: ss
            server: server.name
            port: 443
            cipher: aes-256-gcm
            password: "password"
          - name: "My Vmess Proxy"
            type: vmess
            server: another.server
            port: 443
            uuid: "your-uuid"
            alterId: 0
            cipher: auto
            tls: true
        ```

4.  **Start Mihomo**: Run Mihomo, and it will load your new configuration.




