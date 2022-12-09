# Goal

The goal of this repo is to override Illuminate\Session\Store to use `json_encode` instead of `serialize` (and `json_decode` instead of `unserialize`). This is so we read sessions in other languages such as Node or Golang (`serialize` is not "friendly" to non-PHP languages, even though there's implementations in most languages)

## Steps

The first I did was to extend the `Illuminate\Session\SessionServiceProvider.php`. It's copied/extended to `app\Session\SessionServiceProvider.php`.

So far, so good.

If you uncomment line 7, `use Illuminate\Session\SessionManager;` so it uses our implementation, things starts to go haywire. In `SessionManager.php`, all we do is override `buildSession` & `buildEncryptedSession` so that it uses our implementation of Store, which uses `json_encode` instead of `serialize`. 

And just like that, CSRF token are not appearing anymore. Sessions gets written to the database. It also gets read properly, but somewhere along the line, it doesn't like the fact that I'm overriding SessionManager. 

