# eaclawAI

Local sovereign AI development environment — `*.eaclaude.ai`

## TLS Setup
- Wildcard cert: `certificates/eaclaude.ai.crt` (mkcert, valid Jun 2028)
- Private key: **not committed** — regenerate with `mkcert eaclaude.ai "*.eaclaude.ai"`
- DNS: dnsmasq wildcard via `/etc/dnsmasq.d/eaclaude.conf`

## Regenerate cert
```bash
cd certificates/
mkcert eaclaude.ai "*.eaclaude.ai"
mv eaclaude.ai+1.pem eaclaude.ai.crt
mv eaclaude.ai+1-key.pem eaclaude.ai.key
chmod 600 eaclaude.ai.key
```
