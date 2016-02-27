Adding a server  
`/server add <name_you_want> <server>[/<port> -ssl]`  
example  
`/server add freenode chat.freenode.net/6697 -ssl`  

Connecting to a server  
`/connect freenode` if you added one  
`/connect <server>` if not added  

Changing nick  
`/nick <what_ever>` -> this is the name shown to others  

Joining a channel  
`/join #<channel>`  

Some channels like #archlinux require registration with a third party server to join, like NickServ  
After your nick is registered on NickServ or other, identify:  
`/msg NickServ identify <passwd>`  

This is all fine, but if you want to set a nick and connect automatically to servers  
and channels, you can set options to a specific server.  
To see the server options that can be changed:  
`/server listfull`  



To split screen

 * Horizontaly: `/window splith`
 * Verticaly: `/window splitv`

Bare display: Alt-L

Colored text
`^Cc12^Cbhello ^Cb^Cc04^C_everybody^C_^Cc!`

Add spell check
`/set aspell.check.default_dict "en"`
`/aspell enable`   or   `Alt-s`

File transfer or direct chat
`/dcc chat <nick>`
`/dcc send <nick> <file>`

Remove voice from nick  
`/devoice <nick>`  
`/ignore list`  
`/ignore add <nick>`  
`/ignore del <num> -all`  

List servers
`/links <server> <server_mask>`

List channels
`/list <channel1>,<ch2> <server> -re <regex>`

Part channel
`/part <ch1>,<ch2> <msg>`

Private msg
`/query [-server <server>] <nick> <text>`

Quite nick or host
`/quiet <ch> <nick>`

Time of server
`/time <target>`

Trace route server
`/trace <target>`

Info nick is
`/whois`

Info nick was
`/whowas`
