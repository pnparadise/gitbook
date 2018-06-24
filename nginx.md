proxy\_set\_header  X-Real-IP  $remote\_addr;

proxy\_set\_header Host $host;

proxy\_pass   http://47.104.222.160:8080;

