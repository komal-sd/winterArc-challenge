# Incident Report: Nginx 502 Bad Gateway

## 1. Problem
When I stopped the Python HTTP server running on port 3000,
requests to http://day7.local returned a 502 Bad Gateway error.

Nginx was still running, but the application behind it was unavailable.

## 2. Evidence

- Nginx was running.
- The ss command showed no listening socket on port 3000
  while the Python server was stopped.
- The Nginx error log showed a connection failure to
  http://127.0.0.1:3000/.
- After restarting Python, curl returned HTTP 200 OK.

## 3. Root Cause

The Python HTTP server running on port 3000 was stopped.
Nginx was still running and forwarding requests to that port,
but no application was listening there.

Consequently, Nginx could not connect to its upstream server
and returned a 502 Bad Gateway response.

## 4. Fix

I restarted the Python HTTP server on port 3000.
This restored the upstream application that Nginx forwards
requests to.

## 5. Verification

After restarting the Python server:

- curl returned HTTP 200 OK through Nginx.
- The HTML page was returned successfully.
- The application was accessible again through day7.local.

The incident was resolved.
