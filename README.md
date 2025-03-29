# Arduino Blackmagic SDI Tally REST API
HTTP Bridge for the Blackmagic Arduino Shield to embed SDI Tally Metadata into an SDI Signal by sending HTTP calls to the Arduino Uno with an Ethernet-Shield

## API Endpoints

### Get Status
Get the current state of all cameras:
* [GET /status](http://arduino-ip.local/status)

Example response:
```json
{
    "device": "bmd-sdi-tally",
    "version": "1.0",
    "cameras": [
        {"id": 1, "connected": true, "state": {"program": 1, "preview": 0}},
        {"id": 2, "connected": true, "state": {"program": 0, "preview": 1}},
        {"id": 3, "connected": true, "state": {"program": 0, "preview": 0}},
        {"id": 4, "connected": true, "state": {"program": 0, "preview": 0}}
    ],
    "status": {
        "device_status": "active"
    }
}
```

### Control Tally Preview and Program
Set tally state for a specific camera:
* `GET /tally?cam=[1-4]&pgm=[0,1]&pvw=[0,1]`

  [http://arduino-ip.local/tally?cam=1&pgm=1&pvw=1](http://arduino-ip.local/tally?cam=1&pgm=1&pvw=1)

Parameters:
* `cam`: Camera number (1-4)
* `pgm`: Program state (0=off, 1=on)
* `pvw`: Preview state (0=off, 1=on)

Example URLs:
* Set Camera 1 to Program: [http://arduino-ip.local/tally?cam=1&pgm=1&pvw=0](http://arduino-ip.local/tally?cam=1&pgm=1&pvw=0)
* Set Camera 2 to Preview: [http://arduino-ip.local/tally?cam=2&pgm=0&pvw=1](http://arduino-ip.local/tally?cam=2&pgm=0&pvw=1)
* Turn off Camera 3: [http://arduino-ip.local/tally?cam=3&pgm=0&pvw=0](http://arduino-ip.local/tally?cam=3&pgm=0&pvw=0)

## Using with curl

Get status of all cameras:
```bash
curl http://arduino-ip.local/status
```

Set camera states:
```bash
# Set Camera 1 to Program
curl "http://arduino-ip.local/tally?cam=1&pgm=1&pvw=0"

# Set Camera 2 to Preview
curl "http://arduino-ip.local/tally?cam=2&pgm=0&pvw=1"

# Turn off Camera 3
curl "http://arduino-ip.local/tally?cam=3&pgm=0&pvw=0"
```

## Integration with Companion

This API is compatible with Bitfocus Companion. Use the following settings:
1. Use the Generic HTTP/UDP module
2. For button actions, use the tally endpoint
3. For feedback, poll the status endpoint

## Hardware Setup
* Arduino Uno
* Ethernet Shield
* Blackmagic Arduino Shield
* SDI cables for cameras


## Troubleshooting
* Verify network connectivity by accessing the root page: [http://arduino-ip.local/](http://arduino-ip.local/)
* Check camera connections using the [status endpoint](http://arduino-ip.local/status)
* Monitor the Arduino's serial output for detailed error messages