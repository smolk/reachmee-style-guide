# Python FLASK-restful Style Guide
## Table of contents
1. Basic rules
2. [File structure](#file-structure)
3. Permission model
4. Resources
5. DB Helper
6. Logging
7. [Error handling](#error-handling)

# File structure

``` python
/api
 /restapi
  /admin
   ...
  /commission
   candidates.py # so REST service /api/commission/<int:id>/candidates located here
   ...
  /reports
  extuser.py
  ...
 /dbhelper
```
[back to top](#python-flask-restful-style-guide)

# Error handling
## API Error states
Use `abort(status_code)` to terminate API flow


``` python
# avoid
class ProfileImageWebapi(Resource):
    @token_required
    def get(self, id, filename):

        fullPath = extuser.getPersonalImagePath(id)
        if not fullPath:
            return "error", 404
        ...
```

``` python
# recommended
class ProfileImageWebapi(Resource):
    @token_required
    def get(self, id, filename):

        fullPath = extuser.getPersonalImagePath(id)
        if not fullPath:
            abort(404)
        ...
```

## Facade Error states
Do not return **None**, **False** etc values in case if some single object is not found in DB. Raise custom or generic exception instead

  
``` python
# avoid
def getExtUserByEmail(self, email):
        sql = "SELECT * FROM extuser WHERE email = %(email)s"
        u = db.get(sql, {"email":email.lower()})
        if len(u) == 0:
            return None
        result = u[0]
        return result
```

``` python
# recommended
def getExtUserByEmail(self, email):
        sql = "SELECT * FROM extuser WHERE email = %(email)s"
        u = db.get(sql, {"email":email.lower()})
        if len(u) == 0:
            raise ObjectNotFoundException
        result = u[0]
        return result
```
[back to top](#python-flask-restful-style-guide)