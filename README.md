# Event Server for errors

## Usage

Get golang package __github.com/golang/protobuf/proto__  __github.com/gorilla/mux__	 __github.com/nats-io/go-nats__

Build and run.

`go build eventserver.go;sudo ./eventserver
`

## Access point

This error event server support both unix domain socket and ip:port access. 

- `/var/run/storageos/dataplane-notifications.sock`
- `:1337`

## API definitions
    URL                           HTTP Method  Operation
    /v1/errors                    POST         Post a error event to server
    /v1/errors/stat               GET          Return server event statistics
   
## Issues

One issue is that the original notify-error command demo has en error on id. 

`sudo ./notify-error --domain inet --module director --id 0x00000011 --type eCreateThread --subject config --level 3 --message "Hello this is a test"`   

It actually does not support hex format. __--id 0x00000011__ always return 0. In the source code, __atoi()__ is used so --id 123 would work.

Another issue is notify-error cannot hit a handler through unix socket for this server. It only works with ip:port. net.Listener do get messages from the socket but serverMux seems failed to direct it to a valid handler. If I use following command to send a POST to the server, it will hit the handler.


'echo -e "POST /v1/errors HTTP/1.0\r\n" | sudo netcat -U /var/run/storageos/dataplane-notifications.sock
'

Due to limited amount of time for this project. This issue remains for future investigation. 

## Logs

All log functions has been left enabled for demonstration purpose.

