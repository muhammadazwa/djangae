# Security

Djangae-scaffold provides a good skeleton project setup with various security libraries included
and settings configured as a starting point.

Djangae also provides the following features to aid security:

## Security Middleware

`djangae.contrib.security.middleware.AppEngineSecurityMiddleware` is a Django middleware which
patches certain parts of App Engine and its libraries, specifically:

* It wraps the `fetch` and `make_fetch_call` functions of `google.appengine.api.urlfetch` to make the following changes:
    - The default value of the `validate_certificate` argument is changed from `False` to `True`.
    - If the `url` argument starts with `http` rather than `https` then a warning is logged.  This doesn't block execution.
* The Python `yaml` library is patched so that the default loader is `yaml.loader.SafeLoader` in order to prevent arbitrary Python code execution.
* The Python `json` library is patched so that the default encoder class escapes the HTML entities `<`, `>` and `&`.

This middleware applies the patches and then raises `django.core.exceptions.MiddlewareNotUsed` so that it does not re-apply the patches on subsequent requests.  Note that in tests which don't load any middleware the patches will not be applied.
