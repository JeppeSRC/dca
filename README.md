Forket from [jogramming](https://github.com/jogramming/dca)
====
`dca` is a audio file format that uses opus packets and json metadata.

This package implements a decoder, encoder and a helper streamer for dca v0 and v1.

Usage
===
Encoding
```go

// Encoding a file and saving it to disk
encodeSession := dca.EncodeFile("path/to/file.mp3", dca.StdEncodeOptions)
// Make sure everything is cleaned up, that for example the encoding process if any issues happened isnt lingering around
defer encodeSession.Cleanup()

output, err := os.Create("output.dca")
if err != nil {
    // Handle the error
}

io.Copy(output, encodeSession)
```

Decoding, the decoder automatically detects  dca version aswell as if metadata was available
```go
// inputReader is an io.Reader, like a file for example
decoder := dca.NewDecoder(inputReader)

for {
    frame, err := decoder.OpusFrame()
    if err != nil {
        if err != io.EOF {
            // Handle the error
        }
        
        break
    }
    
    // Do something with the frame, in this example were sending it to discord
    select{
        case voiceConnection.OpusSend <- frame:
        case <-time.After(time.Second):
            // We haven't been able to send a frame in a second, assume the connection is borked
            return
    }
}

```

### Official Specifications
* [DCA Repo](https://github.com/bwmarrin/dca)
* [DCA1 specification draft](https://github.com/bwmarrin/dca/wiki/DCA1-specification-draft)
