# Hosted iFrame Tokenizer

The following entries describe changes to the [Hosted iFrame Tokenizer](?path=docs/documentation/HostediFrameTokenizer.md) and documentation.

Visit status.cardconnect.com and click **subscribe to updates** to receive important release and status notifications.

## Date Updated: 8/20/2021 

The changes below were deployed to the UAT environment on 6/25/2021 and later deployed to the production environment on 7/24/2021.

### New Optional Parameters 

A new `sendcardtypingevent` parameter is available in this release. If **true**, events are sent to the parent page when input to the Card Number field is detected and when the input has completed or timed out.

Additionally, a new `selectinputdelay` parameter is available and intended for mobile implementations where user input unexpectedly causes already populated fields to be cleared.

| Parameter | Default Value | Description |
| --- | --- | --- |
| sendcardtypingevent	false	
If true, an event is sent to the parent page with 'cardTyping':true when the user begins entering a value into the Card Number field. When an onBlur event occurs or the inactivityto value has been met, the card number value is submitted for tokenization and an event is sent to the parent page with 'cardTyping':false.

An example of this event can viewed in the web browser's console on the following example page:
https://fts-uat.cardconnect.com/itoke/outer-page.html?sendcardtypingevent=true

selectinputdelay	0	
Note: This parameter is intended for iOS implementations where user input unexpectedly causes already populated fields to be cleared.

Controls how long (in milliseconds) to ignore input to a newly selected field after changing focus. The default value of 0 represents no delay in accepting input to a newly selected field, whereas the maximum value of 1000 represents a 1 second delay before accepting input to a newly selected field. A value of 100 is typically sufficient to avoid unintended clearing without interfering with the user experience.
