# Hunter (alpha)

[![Hex.pm](https://img.shields.io/hexpm/v/hunter.svg?style=flat-square)](https://hex.pm/packages/hunter)
[![Ebert](https://ebertapp.io/github/milmazz/hunter.svg)](https://ebertapp.io/github/milmazz/hunter)

A Elixir client for [Mastodon](https://github.com/Gargron/mastodon/), a GNU social-compatible micro-blogging service

## Installation

```elixir
def deps do
  [{:hunter, "~> 0.2.0"}]
end
```

Then, update your dependencies:

```sh-session
$ mix deps.get
```

## Usage

Assumming that you already know your *instance* and your *bearer token* you can do
the following:

```elixir
iex(1)> conn = Hunter.new([base_url: "https://example.com", bearer_token: "123456"])
%Hunter.Client{base_url: "https://example.com",
 bearer_token: "123456"}
```

### Getting the current user

```elixir
iex(2)> Hunter.verify_credentials(conn)
%Hunter.Account{acct: "milmazz",
 avatar: "https://social.lou.lt/avatars/original/missing.png",
 created_at: "2017-04-06T17:43:55.325Z", display_name: "Milton Mazzarri",
 followers_count: 2, following_count: 3,
 header: "https://social.lou.lt/headers/original/missing.png", id: 8039,
 locked: false, note: "", statuses_count: 1,
 url: "https://social.lou.lt/@milmazz", username: "milmazz"}
```

### Fetching an account

```elixir
iex(3)> Hunter.account(conn, 8039)
%Hunter.Account{acct: "milmazz",
 avatar: "https://social.lou.lt/avatars/original/missing.png",
 created_at: "2017-04-06T17:43:55.325Z", display_name: "Milton Mazzarri",
 followers_count: 2, following_count: 3,
 header: "https://social.lou.lt/headers/original/missing.png", id: 8039,
 locked: false, note: "", statuses_count: 1,
 url: "https://social.lou.lt/@milmazz", username: "milmazz"}
```

### Getting an account's followers

Returns a list of `Accounts`

```elixir
iex(4)> Hunter.followers(conn, 8039)
[%Hunter.Account{acct: "atmantree@mastodon.club",
  avatar: "https://social.lou.lt/system/accounts/avatars/000/008/518/original/7715529d4ceb4554.jpg?1491509276",
  created_at: "2017-04-06T20:07:57.119Z", display_name: "Carlos Gustavo Ruiz",
  followers_count: 2, following_count: 2,
  header: "https://social.lou.lt/system/accounts/headers/000/008/518/original/394f31473de7c64a.png?1491509277",
  id: 8518, locked: false,
  note: "Programmer, Pythonista, Web Creature, Blogger, C++ and Haskell Fan. Never stop learning, because life never stops teaching.",
  statuses_count: 1, url: "https://mastodon.club/@atmantree",
  username: "atmantree"},
  ...
 ]
```

### Getting who account is following

Returns a list of `Accounts`

```elixir
iex(5)> Hunter.following(conn, 8039)
[%Hunter.Account{acct: "sebasmagri@mastodon.cloud",
  avatar: "https://social.lou.lt/system/accounts/avatars/000/007/899/original/19b4d8c1e9d4e68a.jpg?1491498458",
  created_at: "2017-04-06T17:07:38.912Z",
  display_name: "Sebastián Ramírez Magrí", followers_count: 2,
  following_count: 1,
  header: "https://social.lou.lt/system/accounts/headers/000/007/899/original/missing.png?1491498458",
  id: 7899, locked: false, note: "", statuses_count: 2,
  url: "https://mastodon.cloud/@sebasmagri", username: "sebasmagri"},
  ...]
 ```

### Following a remote user

```elixir
iex(6)> Hunter.follow_by_uri(conn, "paperswelove@mstdn.io")
%Hunter.Account{acct: "paperswelove@mstdn.io",
 avatar: "https://social.lou.lt/system/accounts/avatars/000/007/126/original/60ecc8225809c008.png?1491486258",
 created_at: "2017-04-06T13:44:18.281Z", display_name: "Papers We Love",
 followers_count: 1, following_count: 0,
 header: "https://social.lou.lt/system/accounts/headers/000/007/126/original/missing.png?1491486258",
 id: 7126, locked: false,
 note: "Building Bridges Between Academia and Industry\r\n\r\n<a href=\"http://paperswelove.org\" rel=\"nofollow noopener\"><span class=\"invisible\">http://</span><span class=\"\">paperswelove.org</span><span class=\"invisible\"></span></a>\r\n<a href=\"http://pwlconf.org\" rel=\"nofollow noopener noopener\"><span class=\"invisible\">http://</span><span class=\"\">pwlconf.org</span><span class=\"invisible\"></span></a>",
 statuses_count: 1, url: "https://mstdn.io/@paperswelove",
 username: "paperswelove"}
 ```

### Muting/unmuting an account

```elixir
iex(7)> Hunter.mute(conn, 7899)
%Hunter.Relationship{blocking: false, followed_by: false, following: true,
 muting: true, requested: false}
iex(8)> Hunter.unmute(conn, 7899)
%Hunter.Relationship{blocking: false, followed_by: false, following: true,
 muting: false, requested: false}
```

### Getting an account's statuses

```elixir
iex(9)> Hunter.statuses(conn, 8039)
[%Hunter.Status{account: %Hunter.Account{acct: "milmazz",
   avatar: "https://social.lou.lt/avatars/original/missing.png",
   created_at: "2017-04-06T17:43:55.325Z", display_name: "Milton Mazzarri",
   followers_count: 4, following_count: 4,
   header: "https://social.lou.lt/headers/original/missing.png", id: 8039,
   locked: false, note: "", statuses_count: 2,
   url: "https://social.lou.lt/@milmazz", username: "milmazz"},
  application: %Hunter.Application{client_id: nil, client_secret: nil, id: nil},
  content: "<p>Hunter is a Elixir client for Mastodon: <a href=\"https://github.com/milmazz/hunter\" rel=\"nofollow noopener\" target=\"_blank\"><span class=\"invisible\">https://</span><span class=\"\">github.com/milmazz/hunter</span><span class=\"invisible\"></span></a> <a href=\"https://social.lou.lt/tags/myelixirstatus\" class=\"mention hashtag\">#<span>myelixirstatus</span></a></p>",
  created_at: "2017-04-08T04:41:38.643Z", favourited: nil, favourites_count: 1,
  id: 118635, in_reply_to_account_id: nil, in_reply_to_id: nil,
  media_attachments: [], mentions: [], reblog: nil, reblogged: nil,
  reblogs_count: 0, sensitive: nil, spoiler_text: "",
  tags: [%Hunter.Tag{name: "myelixirstatus",
    url: "https://social.lou.lt/tags/myelixirstatus"}],
  uri: "tag:social.lou.lt,2017-04-08:objectId=118635:objectType=Status",
  url: "https://social.lou.lt/@milmazz/118635", visibility: "public"},
  ...
]
```

### Fetching a user's favourites

```
iex(10)> Hunter.favourites(conn)
[]
```

### Favouriting/unfavouriting a status

```elixir
iex(11)> Hunter.favourite(conn, 442)
%Hunter.Status{account: %Hunter.Account{acct: "FriendlyPootis",
  avatar: "https://social.lou.lt/system/accounts/avatars/000/000/034/original/565da0399c2c26cf.jpg?1491228302",
  created_at: "2017-04-03T13:50:06.485Z", display_name: "FriendlyPootis 🚉",
  followers_count: 62, following_count: 53,
  header: "https://social.lou.lt/system/accounts/headers/000/000/034/original/b009ddb5a8ce41c1.jpg?1491228302",
  id: 34, locked: false,
  note: "fermé comme un carré, Vladimir Pootin sur YT (<a href=\"https://www.youtube.com/VladimirPootin\" rel=\"nofollow noopener\" target=\"_blank\"><span class=\"invisible\">https://www.</span><span class=\"\">youtube.com/VladimirPootin</span><span class=\"invisible\"></span></a>)",
  statuses_count: 253, url: "https://social.lou.lt/@FriendlyPootis",
  username: "FriendlyPootis"},
 application: %Hunter.Application{client_id: nil, client_secret: nil, id: nil},
 content: "<p>les gens pensez à migrer d&apos;instance pour en aller sur une moins chargée tant que vous pouvez, plus vous attendrez plus vous aurez la flemme</p>",
 created_at: "2017-04-03T16:22:04.286Z", favourited: true, favourites_count: 5,
 id: 442, in_reply_to_account_id: nil, in_reply_to_id: nil,
 media_attachments: [], mentions: [], reblog: nil, reblogged: false,
 reblogs_count: 4, sensitive: false, spoiler_text: "", tags: [],
 uri: "tag:social.lou.lt,2017-04-03:objectId=442:objectType=Status",
 url: "https://social.lou.lt/@FriendlyPootis/442", visibility: "public"}
 ```

 ```elixir
 iex(12)> Hunter.unfavourite(conn, 442)
%Hunter.Status{account: %Hunter.Account{acct: "FriendlyPootis",
  avatar: "https://social.lou.lt/system/accounts/avatars/000/000/034/original/565da0399c2c26cf.jpg?1491228302",
  created_at: "2017-04-03T13:50:06.485Z", display_name: "FriendlyPootis 🚉",
  followers_count: 62, following_count: 53,
  header: "https://social.lou.lt/system/accounts/headers/000/000/034/original/b009ddb5a8ce41c1.jpg?1491228302",
  id: 34, locked: false,
  note: "fermé comme un carré, Vladimir Pootin sur YT (<a href=\"https://www.youtube.com/VladimirPootin\" rel=\"nofollow noopener\" target=\"_blank\"><span class=\"invisible\">https://www.</span><span class=\"\">youtube.com/VladimirPootin</span><span class=\"invisible\"></span></a>)",
  statuses_count: 253, url: "https://social.lou.lt/@FriendlyPootis",
  username: "FriendlyPootis"},
 application: %Hunter.Application{client_id: nil, client_secret: nil, id: nil},
 content: "<p>les gens pensez à migrer d&apos;instance pour en aller sur une moins chargée tant que vous pouvez, plus vous attendrez plus vous aurez la flemme</p>",
 created_at: "2017-04-03T16:22:04.286Z", favourited: true, favourites_count: 5,
 id: 442, in_reply_to_account_id: nil, in_reply_to_id: nil,
 media_attachments: [], mentions: [], reblog: nil, reblogged: false,
 reblogs_count: 4, sensitive: false, spoiler_text: "", tags: [],
 uri: "tag:social.lou.lt,2017-04-03:objectId=442:objectType=Status",
 url: "https://social.lou.lt/@FriendlyPootis/442", visibility: "public"}
 ```

### Get instance information

```elixir
iex(13)> Hunter.instance_info(conn)
%Hunter.Instance{description: "Mostly French  instance - <a href=\"/about/more#rules\">Read full description</a> for rules.",
 email: "maxime+mastodon@melinon.fr", title: "Loultstodon",
 uri: "social.lou.lt"}
```

### Fetch user's notifications

```elixir
iex(14)> Hunter.notifications(conn)
[%Hunter.Notification{account: %Hunter.Account{acct: "paperswelove@mstdn.io",
   avatar: "https://social.lou.lt/system/accounts/avatars/000/007/126/original/60ecc8225809c008.png?1491486258",
   created_at: "2017-04-06T13:44:18.281Z", display_name: "Papers We Love",
   followers_count: 1, following_count: 1,
   header: "https://social.lou.lt/system/accounts/headers/000/007/126/original/missing.png?1491486258",
   id: 7126, locked: false,
   note: "Building Bridges Between Academia and Industry\n\n<a href=\"http://paperswelove.org\" rel=\"nofollow noopener\"><span class=\"invisible\">http://</span><span class=\"\">paperswelove.org</span><span class=\"invisible\"></span></a>\n<a href=\"http://pwlconf.org\" rel=\"nofollow noopener noopener\"><span class=\"invisible\">http://</span><span class=\"\">pwlconf.org</span><span class=\"invisible\"></span></a>",
   statuses_count: 8, url: "https://mstdn.io/@paperswelove",
   username: "paperswelove"}, created_at: "2017-04-08T12:15:53.467Z", id: 17476,
  status: nil, type: "follow"},
 ...
]
```

### Fetch a single notification

```elixir
iex(15)> Hunter.notification(conn, 17476)
%Hunter.Notification{account: %Hunter.Account{acct: "paperswelove@mstdn.io",
  avatar: "https://social.lou.lt/system/accounts/avatars/000/007/126/original/60ecc8225809c008.png?1491486258",
  created_at: "2017-04-06T13:44:18.281Z", display_name: "Papers We Love",
  followers_count: 1, following_count: 1,
  header: "https://social.lou.lt/system/accounts/headers/000/007/126/original/missing.png?1491486258",
  id: 7126, locked: false,
  note: "Building Bridges Between Academia and Industry\n\n<a href=\"http://paperswelove.org\" rel=\"nofollow noopener\"><span class=\"invisible\">http://</span><span class=\"\">paperswelove.org</span><span class=\"invisible\"></span></a>\n<a href=\"http://pwlconf.org\" rel=\"nofollow noopener noopener\"><span class=\"invisible\">http://</span><span class=\"\">pwlconf.org</span><span class=\"invisible\"></span></a>",
  statuses_count: 8, url: "https://mstdn.io/@paperswelove",
  username: "paperswelove"}, created_at: "2017-04-08T12:15:53.467Z", id: 17476,
 status: nil, type: "follow"}
```

### Clear notifications

```elixir
iex(16)> Hunter.clear_notifications(conn)
"{}"
iex(17)> Hunter.notifications(conn)
[]
```

### Get a card associated with a status

```elixir
iex(18)> Hunter.card_by_status(conn, 118635)
%Hunter.Card{description: "hunter - A Elixir client for Mastodon, a GNU Social compatible micro-blogging service",
 image: "https://social.lou.lt/system/preview_cards/images/000/000/378/original/34700?1491626499",
 title: "milmazz/hunter", url: "https://github.com/milmazz/hunter"}
```

### Fetch a list of follow requests

```elixir
iex(19)> Hunter.follow_requests(conn)
[]
```

### Fetch user's blocks

```elixir
iex(20)> Hunter.blocks(conn)
[]
```

### Fetch user's mutes

```elixir
iex(21)> Hunter.mutes(conn)
[]
```

### Fetch user's reports

```elixir
iex(22)> Hunter.reports(conn)
[]
```

## TODO

* OAuth2 authentication
  - Register client for token-access
  - Token authentication for API usage
* Register an application
* Authorizing or rejecting follow requests
* Support arrays as parameter type
* Uploading media attachment
* Getting who reblogged/favourited a status
* Verify each endpoint
* Improve unit tests
* Improve documentation

## License

Hunter source code is released under Apache 2 License.

Check the [LICENSE](LICENSE) for more information.
