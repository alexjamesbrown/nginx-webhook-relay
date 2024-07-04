# NGINX Webhook Relay

This simple nginx config aims to solve a fairly simple problem:  
- I have a third party which emits webhooks. 
- It can only deliver those webhooks to one url.
- I need to deliver those requests to two (or more) endpoints simultaneously.

The config here simply forwards ALL requests to 2 urls. No filtering, no validation.  
It doesn't do anything with https, and only listens on port 80 (for simplicity of this demo)

Makes use of [ngx_http_mirror_module](https://nginx.org/en/docs/http/ngx_http_mirror_module.html)

## Set your mirror URLs:
In `nginx.conf` set your locations. I've gone with `/v1` and `/v2` but this could be `/live` and `/test` for example.

For simplicity, I'm using [Request Bin](https://pipedream.com/requestbin)  
(create your own bins - mine have probably been deleted)

The `/accepted` location is just there because the `ngx_http_mirror_module` requires an initial proxy_pass to forward requests to.

### Run Docker Container

```docker run --name webhook-relay -p 8081:80 -v $(pwd)/nginx.conf:/etc/nginx/nginx.conf:ro -d nginx:latest```

### Make HTTP requests
There's a `test.http` file in this repo. Run that, and it'll forward requests to both locations.