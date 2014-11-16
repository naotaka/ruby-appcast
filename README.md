ruby-appcast
============

Ruby appcast.xml generator for Sparkle Framework

Tested with Ruby 2.1

### Very simple (hard-coded) example

```ruby
#!/usr/bin/env ruby
  
require 'rexml/document'
require_relative 'appcast'

release_notes = [
  Screenhero::Sparkle::AppCast::Item::ReleaseNotesLink.new("en", "http://myawesomeapp.com/update/en/release_notes.html"),
  Screenhero::Sparkle::AppCast::Item::ReleaseNotesLink.new("ja", "http://myawesomeapp.com/update/ja/release_notes.html"),
]

appcast = Screenhero::Sparkle::AppCast.new(name: "MyAwesomeApp",
    description: "MyAwesomeApp updates",
    dsa_priv: File.open("/path/to/dsa_priv.pem", File::RDONLY).read,
    appcast_url: "http://myawesomeapp.com/update/sparkle.xml")
appcast.add_item("1.0.1",
    "/path/to/MyAwesomeApp-1.0.1.dmg",
    "http://myawesomeapp.com/update/MyAwesomeApp-1.0.1.dmg",
    release_notes,
    "10.9")
  
puts appcast.to_s
```

### Example Output:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<rss xmlns:sparkle='http://www.andymatuschak.org/xml-namespaces/sparkle' xmlns:atom='http://www.andymatuschak.org/xml-namespaces/sparkle' version='2.0'>
  <channel>
    <title>MyAwesomeApp</title>
    <description>MyAwesomeApp updates</description>
    <language>en</language>
    <pubDate>Thu, 07 Aug 2014 22:12:57 -0700</pubDate>
    <link>http://myawesomeapp.com/update/sparkle.xml</link>
    <atom:link type='application/rss+xml' href='http://myawesomeapp.com/update/sparkle.xml'/>
    <item>
      <title>MyAwesomeApp 1.0.1</title>
      <sparkle:releaseNotesLink xml:lang='en'>
        http://myawesomeapp.com/update/en/release_notes.html
      </sparkle:releaseNotesLink>
      <sparkle:releaseNotesLink xml:lang='ja'>
        http://myawesomeapp.com/update/ja/release_notes.html
      <sparkle:minimumSystemVersion>10.9</sparkle:minimumSystemVersion>
      <pubDate>Thu, 07 Aug 2014 22:12:57 -0700</pubDate>
      <guid isPermaLink='false'>MyAwesomeApp 1.0.1</guid>
      <enclosure type='application/dmg' sparkle:version='1.0.1' length='1031984' sparkle:dsaSignature='MEQCIDbfctm3pCqJw2SSbZGHyrH3XKtvsw/VICvAHVB3CBRsAiAcd6RDoRwi
8ACxEHsNGeepi5A0dZV0KOXT4pJm70AqoA==
' url='http://myawesomeapp.com/update/MyAwesomeApp-1.0.1.dmg'/>
    </item>
  </channel>
</rss>
```
