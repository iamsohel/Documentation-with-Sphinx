Users
=======

This is users api documentations.

.. toctree::
:maxdepth: 2
            :glob:

Attributes
^^^^^^^^^^^^^

users
-------

1. id
2. first_name
3. last_name
4. email
5. password
6. active
7. activation_code
8. reset_code
9. login_platform
10. social_data
11. last_login
12. created
13. modified

user_role
----------

1. id
2. user_id
3. role_id
4. created
5. modified

role
-----

1. id
2. name
3. description
4. created
5. modified

Functions
^^^^^^^^^^^

login
------
This is the user authentication function. User will provide his email and password to login this system. If he gives valid
email and password, system will generate a access token for this user and return the user information like user id, name, email,
user role, and access token.

.. http:post:: /login


    **Example request**:

    .. sourcecode:: http

      post /users/login/
      parameters : email, password
      Accept: text/javascript

    **Example response**:

    .. sourcecode:: http

      Content-Type: application/json

      [
        {
          "status": OK,
          "result":{
                     "data":{
                            "token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJlbWFpbCI6InNyYW5hQGxlYXBpbmdsb2dpYy5jb20ifQ.Ui3XrvnuXg8OjFW9xk8BD3F0aG9HlgjYwQOEOYu5sLw",
                            "user_id":"1",
                            "name":Sohel Rana,
                            "email":"srana@leapinglogic.com",
                            "user_role": "Admin, Developer"
                            },
                     "message": "Logged in successfully."
                  }
        }
      ]

    :statuscode 200: OK
        :statuscode 400: NOK, error : bad request


forgotPassword
---------------
If user forgot his/her password, he will provide his email to reset his password. When he will give password, system will
check that users email is existed in database or not. If exists, system will send a email to that email address with a reset
password link and activation_code. When user click that link, he can reset password.

.. http:post:: /forgotPassword


    **Example request**:

    .. sourcecode:: http

      post /users/forgotPassword/
      parameters : email
      Accept: text/javascript

    **Example response**:

    .. sourcecode:: http

      Content-Type: application/json

      [
        {
          "status": OK,
          "result":{
                     "message": "An email has been sent with reset password link."
                   }
        }
      ]

    :statuscode 200: OK
        :statuscode 400: NOK, error : bad request
        :statuscode 500: internal server error, when email will not sent


resetPassword
---------------
If user forgot his password, user will give his email address. System will send a reset password link to that email
address. When user will click the reset password link, he can reset his password.

.. http:post:: /resetPassword


    **Example request**:

    .. sourcecode:: http

      post /users/resetPassword/
      parameters : activation_code
      Accept: text/javascript

    **Example response**:

    .. sourcecode:: http

      Content-Type: application/json

      [
        {
          "status": OK,
          "result":{
                     "message": "Password has been changed successfully."
                   }
        }
      ]


    :statuscode 200: OK
        :statuscode 400: NOK, error : bad request

verifyEmail
------------
When user submit a signup form, an email will be sent to that submitted email address with account activation link. If user
provides valid email, he will get that activation lin. After clicking the activation link, user can activate his account.

.. http:post:: /verifyEmail


    **Example request**:

    .. sourcecode:: http

      post /users/verifyEmail/
      parameters : email, activation_code
      Accept: text/javascript

    **Example response**:

    .. sourcecode:: http

      Content-Type: application/json

      [
        {
          "status": OK,
          "result":{
                     "message": "An email is sent to your email address. Please check your email and activate account."
                   }
        }
      ]


    :statuscode 200: OK
        :statuscode 400: NOK, error : bad request
        :statuscode 500: internal server error, when email will not sent


activate
------------
When user clicks the account activation link from his email, this will check first this activation code is valid or not. If valid
then it will check the account activation time is expired or not. By default after 7 days from sending activaion link will expired the
account activation time. After verifing account activation time, then it will set the provided password and  the account will be activated.

.. http:post:: /activate/activation_code


    **Example request**:

    .. sourcecode:: http

      post /users/activate/
      parameters : activation_code, password
      Accept: text/javascript

    **Example response**:

    .. sourcecode:: http

      Content-Type: application/json

      [
        {
          "status": OK,
          "result":{
                     "message": "Your account has been activated successfully. Please login to your account."
                   }
        }
      ]


    :statuscode 200: OK
        :statuscode 400: NOK, error : bad request


checkEmail
------------
To check user's entered email is already existed in database or not. If exists, system will show message your entered email
is already existed in database. Please give another unique email.

.. http:post:: /checkEmail


    **Example request**:

    .. sourcecode:: http

      post /users/checkEmail/
      parameters : email
      Accept: text/javascript

    **Example response**:

    .. sourcecode:: http

      Content-Type: application/json

      [
        {
          "status": OK,
          "result":{
                     "message": "This email is not existed."
                   }
        }
      ]


    :statuscode 200: OK
        :statuscode 400: NOK, error : bad request


loggedinUser
--------------
Get logged in user's details from token. It will fetch the user and profile informations.

.. http:get:: /loggedinUser


    **Example request**:

    .. sourcecode:: http

      get /users/loggedinUser/
      parameters : token
      Accept: text/javascript

    **Example response**:

    .. sourcecode:: http

      Content-Type: application/json

      {
            "status": "OK",
            "result": {
                "payload": {
                    "email": "arakib@leapinglogic.com"
                },
                "data": {
                    "id": 2,
                    "first_name": "Rakib",
                    "last_name": "Abdullah",
                    "name": "Rakib Abdullah",
                    "email": "arakib@leapinglogic.com",
                    "phone": "3425432544",
                    "image": "",
                    "home_phone": "0345327534",
                    "mobile": "04351978843978",
                    "fax": "43534253534",
                    "facebook": "",
                    "google": "",
                    "linkedin": "",
                    "twitter": "",
                    "active": null
                }
            }
      }

    :statuscode 200: OK
        :statuscode 403: NOK, error : No request data has found


changePassword
----------------
Change user password.

.. http:post:: /changePassword/id


    **Example request**:

    .. sourcecode:: http

      post /users/changePassword/id
      parameters : token, id,  password
      Accept: text/javascript

    **Example response**:

    .. sourcecode:: http

      Content-Type: application/json

      {
          "status": "OK",
          "result": {
                      "message": "Password has been changed successfully."
                    }
      }

    :statuscode 200: OK
        :statuscode 404: NOK, error : provided old_password and database password doesn't match
        :statuscode 403: NOK, error : No request data has found


updateAvatar
----------------
Update user avatar.

.. http:post:: /updateAvatar/id


    **Example request**:

    .. sourcecode:: http

      post /users/updateAvatar/
      parameters : token, id, image file
      Accept: text/javascript

    **Example response**:

    .. sourcecode:: http

      Content-Type: application/json

      {
          "status": "OK",
          "result": {
                      "message": "Avatar has been updated successfully."
                    }
      }

    :statuscode 200: OK
        :statuscode 404: NOK, error : Avatar could not updated. Please try again.
        :statuscode 403: NOK, error : 'No request data has found. Please upload avatar.


find
----------
Search users by first_name or last_name, email

.. http:get:: /find?search=sohel


    **Example request**:

    .. sourcecode:: http

      get /users/find?search=sohel
      parameters : token, search
      Accept: text/javascript

    **Example response**:

    .. sourcecode:: http

      Content-Type: application/json

      {
         "status": "OK",
         "result": {
                "data":
                [
                    {
                        "id" : 1
                        "first_name": "Rakib",
                        "last_name": "Abdullah",
                        "email": "arakib@leapinglogic.com"
                    }
                ]
         }
      }

    :statuscode 200: OK
        :statuscode 403: NOK, error : 'No request data has found.

