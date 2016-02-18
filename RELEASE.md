# AT&T Enhanced WebRTC JS SDK Release Notes

This SDK includes the following components:

* JavaScript library - a client library to access the AT&T Enhanced WebRTC API.
* Node.js sample app - a Web app demonstrating the features of the JavaScript library and managing app configuration elements such as App Key and App Secret. **Please refer to `node-sample/RELEASE.md` for sample app-specific notes**.
* Developer Hosted Server (DHS) library - a Node.js library for generating OAuth access tokens and E911 IDs. **Please refer to  `node-sample/lib/RELEASE.md` for DHS library-specific notes**.

## Features of the JavaScript library

The following features and functionality are available in the current SDK release for all three supported calling types (Virtual Number and Account ID):

### Chrome

*	**Basic audio and video call management** – make, receive, answer, end, mute, unmute, hold, resume, cancel, and reject calls.
*	**Basic audio and video conferencing** – create a conference, add and remove participants, hold, resume, mute, unmute, and end conference.
  *	Supports dialing out to add participants.
*	**Advanced call management** – move, transfer, add a second call, switch between two calls, upgrade, downgrade, reject downgrade and reject upgrade requests.
  *	Audio and video calls can be moved Web-to-Web
  *	Calls can be moved from the Web to an AT&T mobile phone

####  Chrome v47 Impact
  Google released Chrome 47 on Dec 1, 2015. This version introduced the following changes that break backwards-compatibility for WebRTC API.
  1. getUserMedia now requires HTTPS
  2. MediaStream.stop removed
  3. Changes in SDP processing

**What you need to do**
  1. getUserMedia requires HTTPS

  `ACTION NEEDED:`

  Please ensure your Web Application is always used from HTTPS URL. If your user tries to use your app from HTTP URL, it should be automatically redirected to equivalent HTTPS URL.

  2. MediaStream.stop removed

  Following Phone features will not work in Chrome v47 if you continue to use ewebrtc-sdk versions 1.0.5 or earlier.

  Methods: `Phone.hangup(), Phone.endConference()`

  Events:  `call:disconnected, conference:ended`

  `ACTION NEEDED:`

  Please update the ewebrtc-sdk JavaScript library in your HTML pages to the latest version v1.0.6.

  Latest version is available at https://raw.githubusercontent.com/attdevsupport/ewebrtc-sdk/master/js/ewebrtc-sdk.min.js.

  You don't have to make any other changes to your application code.

  ewebrtc-sdk v1.0.6 will also work correctly with Chrome v46 even if your users did not upgrade their browsers yet.

  3. Changes in SDP processing

  `NO ACTION NEEDED:`

  If you are using ewebrtc-sdk library, you are not impacted by this.


##### How to find out the version of ewebrtc-sdk your Web App is using

* Visit your Web App URL in Chrome
* Go to a web page that uses `Phone` methods from `ewebrtc-sdk` library
* Right-click anywhere on the page. In the pop-up menu, click `Inspect`
* In the DevTools window, click `Console`
* In Console, type the command: `ATT.rtc.Phone.getPhone().getVersion()`. It should show `1.0.6`

### Firefox

  *	**No SDK features are tested with Firefox.**

### Upcoming features

The following features will be added soon:

* Firefox browser support
* AT&T Mobile Number support

# v1.0.6                                     

December 4, 2015

* **Fix:** `MediaStream.stop()` is deprecated and is removed in Chrome v47.
* **Fix:** Hold-resume by caller followed by another hold resume by callee results in no media.
* **Fix:** SDK failed to refresh session after it gets expired.

## Known Issues

### SDK Issues

#### Basic Call
* No audio from Callee when resuming a call after upgrade by caller. 
* Redirect uri with invalid value needs better error message. 
* Auto resume fails when dialing a second Call and call was canceled or rejected. 

#### Basic Conference  
* While in a conference, creating a 2nd conference doesn’t generate an error. 
* During conference if user logs out `disconnectConference` method is not getting called. 

#### Move
* Move from the callee side has one way audio video when done Twice. 
* When I initiate move, my devices will start ringing, but if the other participant disconnects, I never get notified and my devices keep ringing. 
* One Way Audio after successfully transferring video call from Browser to ICMN handset. 

#### Transfer
* Camera not released after Transfer Successful. 

#### Downgrade/Upgrade
* After Rejecting upgrade, holding then resuming results in no video. 
* After Rejecting downgrade, holding then resuming results in no video. 

### Platform  Defects :

#### Impacted scenarios due to Conference Issue
* When any user started a conference and invites a Mobile Device user and the user rejects the invitation. I don't get
invitation rejected event. 
* Adding Mobile Device as a participant to a video conference results in one way audio. 
* When a participant leaves a conference by using the `endConference` method, the platform does not generate
  the necessary event to inform the host.


#### Impacted scenarios due to SSL role Issue
* Hold-resume from callee fails after upgrade from Callee. 
* Ssl role error when trying to upgrade from callee side after callee rejected the previous upgrade from caller. 
* Reject downgrade from Callee fails with ssl role. 
* Reject from Caller fails when call was previously upgraded by callee. 
* Hold,Move failed from caller side after successful upgrade from callee. 
* Upgrade from Callee after successful reject(upgrade) Callee fails with ssl-role error. 
* Upgrade , Hold, resume, move fails with ssl-role error after Reject a downgrade from caller. 
* Upgrade from caller fails after upgrade request was rejected by caller.  
* Move/Hold Call from callee fails after successful upgrade reject from caller side. 
* SSL role error when callee puts caller on hold after successful upgrade. 
* SSL role error when accepting media modification initiated by the callee. 

#### Impacted scenarios due to Ice-ufrag Issue
* Hold from callee throws ice-ufrag after downgrade from caller / from callee. 
* Upgrade Callee after Downgrade Callee throws an ice-ufrag error. 
* Reject , upgrade doesn't work from the callee side after downgrade from caller. 
* Hold after reject from callee fails. 
* Modifying call multiple times from the callee side throws the error: Failed to set remote offer SDP: Called with SDP without ice-ufrag and ice-pwd. 
* Resume from transferee results in transfer target throwing error: Failed to set remote offer SDP: Called with SDP without ice-ufrag and ice-pwd. 


#### Impacted scenario due to Move Issue
* After Successful downgrade , move from the callee show as Downgrade request. 
* Move caller after downgrade caller fails. 
* If a call originates from Virtual Number or Account ID, and a call move is attempted by the caller, the call will drop. If the call is moved by the recipient, the move is successful. 

#### Media Issues
* Video streaming issues when the caller downgrades then upgrades the call. 
* Video switching between participants is sometimes inconsistent. Turn off your microphone while the other participant speaks, that will switch the video to the speaking participant.


#### IPv6 media Issue
* Media stream may not function correctly unless IPv6 is disabled. The default behavior is to use IPv4, so the developer doesn't need to explicitly disable IPv6. 


## Notes

* Establishing calls with Firefox is notably slower than doing so with Chrome.
* Transfer to a non-provisioned Mobile Device target fails while in video Call. The transfer fails with error: `There
 was an error transferring the call. Forbidden`.  (Platform unsupported)

## Tested Environments

* Chrome Version v47.0 for OSX v10.11.1 and Windows 8

**_The SDK may also work for other operating systems and browsers but not tested or supported._**


# Changelog

## v1.0.4                                       

October 12, 2015

* **New:** Handle Network Disconnection and Reconnection.
* **New:** connectivity:on and connectivity:off event for network detection.

## v1.0.3

September 11, 2015

* **Fix:** When rejecting conference invitation host is getting the 'unrecognized error' notification.
* **Fix:** SDK doesn't fire `call:rejected` event for the caller.
* **Fix:** Adding participant to conference logs __"Successfully added participant"__ instead of __"Successfully sent invitation to participant"__.
* **Fix:** List of participants is not cleared when ending a conference.
* **Fix:** Audio feedback loop when making calls in v1.0.2.

## v1.0.2

August 28, 2015

* **Fix:** The method `phone.on` throws an error instead of publishing one.
* **Fix:** GitHub hosted tutorial documentation links are broken.
* **Fix:** No timestamp for error messages.
* **Fix:** No documentation for transfer events: `call:transferring` and `call:transferred`.
* **Fix:** `call:hold` and `call:resume` events not triggered when caller/callee does hold and resume more than once.

## v1.0.1

August 21, 2015

* **Fix:** Standardize errors that SDK publishes
* **Fix:** Add SDK error codes to the API docs
* **Fix:** Add all API errors to SDK - API error dictionary.
* **Fix:** API error not caught for SVC001 when dialing yourself as account ID user.
* **Fix:** Publish `call:resumed` event for the party who put on hold.
* **Fix:** Auto resume fails when dialing a second call and call was canceled or rejected.
* **Fix:** There is no timestamp on `error` and `session:ready` events.
* **Fix:** One way media after transferring users with video media to target users with audio media.
* **Fix:** Callee user on the browser is not publishing `media:established` event.

## v1.0.0

August 14, 2015

* **Fix:** Move from the callee side has one way audio video when done Twice.
* **New:** New method `Phone.getVersion` to retrieve the SDK version.
* **New:** New event 'gateway-unreachable' on Phone to capture '502' and '503' errors.
* **New:** Handling conference invitations when there is an active call or conference.


## v1.0.0-rc.27

July 31, 2015

* **Fix:** Adding a 2nd call doesn't work after putting the first call on hold explicitly.
* **Fix:** Dial method should throw an error when there is an active call.
* **Fix:** Error for phone.acceptModifications should be handled gracefully if no modifications data on the current call.
* **Fix:** Media fails when canceling 2nd outgoing call.
* **Fix:** Media fails if rejecting incoming call while call is in progress.
* **Fix:** Call from first user (already on hold) when second call is cancelled should be resumed successfully with media on both sides.
* **Fix:** Two Way Hold: When caller is on hold, callee cannot put caller on hold.
* **Fix:** Move after been put on hold not working.


## v1.0.0-rc.26

July 22, 2015

* **Fix:** Throw SDK error when trying to send a DTMF tone when call is on hold.
* **Fix:** SDK should put current call on hold while dialing a new number using `phone.dial`.

## v1.0.0-rc.25

July 16, 2015

* **New:** Capability to send DTMF (dialing) tones.
* **New:** API for setting and resetting audio codecs to user during the call.
* **Fix:** Upgrading muted call re-enables audio on both sides.
* **Fix:** Downgrading muted re-enables audio on both sides.
* **Fix:** When moving a call using `Phone.move`, the error message `Unrecognized event state: move-terminated` is printed in the browser console. The move operation is successful.
* **Fix:** Switching to the background call results in no media.
* **Fix:** Cannot hold a muted call.

## v1.0.0-rc.24

July 13, 2015

* **Fix:** One way audio after hold and resume a call from the callee side.
* **Fix:** No error thrown when trying to modify a call that's being held or in hold state.
* **Fix:** No error thrown when trying to move a call that's being held or in hold state.
* **Fix:** When callee in audio-only call puts the caller on hold and tries to move the call, the phone doesn't ring and both users get disconnected.
* **Fix:** When callee in audio-only call puts the caller on hold and caller tries to move the call, the phone doesn't ring and there is no media flow between both.
* **Fix:** Callee is allowed to upgrade in held state.
* **Fix:** Moving the second call will display `connecting` notification after call got connected for a user with 2 calls.
* **Fix:** If Bob puts Alice on hold, if successful the state of Alice's call should be hold.
* **Fix:** Foreground call loses media if background call is hungup by the other party
* **Fix:** SDP Parsing Error: c= connection line not specified for every media level, validation failed. when moving a call to the mobile phone.
* **Fix:** The SDK does not fire `media:establised` event on callee side when answering incoming call.

## v1.0.0-rc.23

July 2, 2015

* **Fix:** Upgrade after downgrade doesn't work due a known issue with the underlying platform.
* **Fix:** Moving Video Call from Browser to Browser fails when in call with a provisioned Mobile Device.
* **Fix:** Transfer from Browser to a Provisioned non-VoLTE Mobile device fails with different reasons.
* **Fix:** When holding a call after successfully downgrading it, the `Phone.hold` method fails with error message: `SVC0001:A service error has occurred. Error code is Parameter 'sdp' has invalid format.,Variables=Parameter 'sdp' has invalid format`.
* **Fix:** When downgrading a call after successfully resuming it, the `Phone.downgrade` method fails with error message: `SVC0001:A service error has occurred. Error code is Parameter 'sdp' has invalid format.,Variables=Parameter 'sdp' has invalid format`.
* **Fix:** Downgrade fails after successful Call Move.
* **Fix:** Recipient can resume a call that has been held by hold initiator.

## v1.0.0-rc.22

June 12, 2015

* **Fix:** Improved documentation on number format when dialing and adding participants to a conference.
* **Fix:** Wrong video media type shown when making calls from the browser to mobile devices.
* **New:** New method `Phone.rejectModification`. This method allows the user to reject media modification requests such as upgrade requests generated by the `Phone.upgrade` method or downgrade requests generated by the `Phone.downgrade` method.
* **New:** Automatically reject a call modification in the sample app if it’s not accepted after 5 seconds.

## v1.0.0-rc.21

June 5, 2015

* **Fix:** One-way audio on non-provisioned VoLTE handset following call transfer.

## v1.0.0-rc.20

June 2, 2015

* **New:** Upgrade an audio call to video call.

## v1.0.0-rc.19

May 29, 2015

* **Fix:** The object returned by `getCallerInfo()` is improperly formatted for Account ID users.
* **Fix:** Sample app shows a username encoded as a number for Account ID users.
* **Fix:** Intermittent `ice-ufrag` error when answering a call.

## v1.0.0-rc.18

May 22, 2015

* **Fix:** Mobile Number users with VoLTE devices are unable to answer incoming calls. The call keeps ringing and eventually goes to voicemail.

## v1.0.0-rc.17

May 15, 2015

* **Fix:** Setting global logging level to trace generates a JavaScript error while making a call.
* **Fix:** Error during transferring a call.
* **Fix:** Video call quality on a call holder's side is extremely poor following resume from hold.
* **Fix:** Transfer to a provisioned phone fails when in a video call with non-provisioned phone.
* **Fix:** Transferring a call fails with an HTTP error code `409` when an Account ID user transfers to a mobile device.


## v1.0.0-rc.16

April 16, 2015

* **New:** New `ATT.logManager` features:
  * `getLogLevels`: Method to get all defined log levels for the log manager.
  * `getCurrentModuleLogLevels`: Method to get the current modules with their defined log levels.
  * `getGlobalLogLevel`: Method to get the current log level for all modules.
  * `setGlobalLogLevel`: Method to set a log level for all the modules.
  * `resetLogLevels`: Method to reset log levels for all modules to their defaults.
  * `getLoggers`: Method to get the log manager's list of modules.
  * `getModuleDefaults`: Method to get the modules with defined default log levels.
  * `createCustomLogger`: Method to create a custom logger by passing a module name.
  * `deleteCustomLogger`: Method to delete a custom logger by passing a module name.
* **Changed:** Changed `ATT.logManager` feature
    * `getLogger`: Method to get the logger object by passing the module name.
* **Removed:** Removed from `ATT.logManager`
  * `addLoggerForModule`
  * `configureLogger`
  * `getLoggerByName`
  * `logLevel`
  * `loggerType`
  * `LOG_LEVEL`
  * `LOGGER_TYPE`
* **New:** New `ATT.logManager.Logger` features:
  * `isCustomType`: Returns true if a custom Logger
  * `setLogLevel`: Sets log level of Logger
  * `getLogLevel`: Gets log level of Logger
* **Removed:** Removed from `ATT.logManager.Logger`
  * `level`
  * `setLevel`
  * `setType`
  * `Type`
* **New:** Methods to manage configuration for `RTCPeerConnection`.
  * `getIceServers`: Function to get the current ICE servers that the SDK uses for creating peer connections.
  * `setIceServers`: Method to set the list of ICE servers that the SDK uses for creating peer connections.
  * `resetIceServers`: Method to reset the list of ICE servers to default list for peer connection configuration.
  * `getIceTransport`: Function to get the current ICE transport value that the SDK uses for creating peer connections.
  * `setIceTransport`: Method to set the ICE transport that the SDK uses for creating peer connections.
  * `resetIceTransport`: Method to reset the ICE transport value to the value for peer connection configuration.
  * `getIpv6`: Function to get the ipv6 value that the SDK uses for creating peer connections.
  * `setIpv6`: Method to set the ipv6 value that the SDK uses for creating peer connections.
  * `resetIpv6`: Method to reset the ipv6 to the default value for peer connection configuration.

## v1.0.0-rc.15

April 3, 2015

* **Fix:** Users cannot log in again using the same Access Token.
* **Fix:** SDK can dial a second call by putting the previous call automatically on hold.
* **Fix:** Video stream is dropped when A video call is on hold for 15 minutes or more before the call is resumed.


## v1.0.0-rc.14

March 22, 2015

* **New:** DHS functions are now available as a public module named `att-dhs`. You can use this as you would a regular node module.
* **Removed:**
 * Stand-alone node-dhs is removed. DHS functions are now available in the `att-dhs` public module. You can use these DHS functions in your own Node.js app. An illustrative example is provided in the /node-sample/ folder.
 * ATT.rtc.dhs namespace is deleted. Use the migration guide for code snippets to invoke sample app DHS routes.
 * The  ATT.rtc.configure method is no longer needed.
* **Removed:**
 * DHS routes are now in /node-sample/.
 * `restify-dhs` is optional, use this only if you want a standalone DHS.
* **Fix:** `phone.logout` method will always return after cleaning a session and call resources, if any.

## v1.0.0-rc.13

March 6, 2015

* **Fix:** Call index is the same for 2 calls in phone.getCalls().
* **New:** DHS Library: A new server-side Node.js based library to provide oAuth token and E911 ID methods.
    * This will replace a separate server for DHS.
    * The DHS-specific configuration (e.g., app key, app secret) can now be done on the sample server itself
    * Please refer to the DHS Library JSDocs and ReadMe for further information.
* **Change:** The following SDK methods are changed. **Please update your Web application accordingly.**
    * ATT.rtc.dhs.createAccessToken is now ATT.rtc.createAccessToken
    * ATT.rtc.dhs.createE911Id is now ATT.rtc.createE911Id
    * ATT.rtc.configure now has a new signature. Please refer to SDK API JSDocs for updates to this method.
* **Change:** The sample app is now launched by navigating to `https://localhost:9001`.
    * You still must add a security exception for `https://localhost:9001` to your browser.
    * Please refer to the ReadMe for more information.



## v1.0.0-rc.12

February 20, 2015

* **Fix:** Multiple Login Failed errors appear in succession, concatenating when the page is not refreshed.
* **Fix:** No audio on handset following a call move.
* **Fix:** Move call unnecessarily fires call:connected and media:established events and throws a JS exception.

* **New:** The Call-id in the event object will be a complete URI instead of parsed numbers or ID.
  * Added the new method getCallerInfo(callerUri) to parse the raw URI to the return information like callerId, protocol, domain, port, and other associated information.


## v1.0.0-rc.11

February 6, 2015

* **Fix:** The DHS and the Sample App now use the IP address `127.0.0.1` instead of the hostname `localhost` so that the developer only has to add one SSL security exception.
* **Fix:** Video camera remains on after disconnecting a call.
* **Fix:** Unable to uniquely track two calls to the same number. QC 74097.
  * Event data associated with a call/conference event now has `index` that refers to ordinal numbering of the call in the list of existing calls.
  * Public method `phone.getCalls` updated to return call `index`, `breed`, `type`, `state` and `peer`/`participants`.
* **New:** The Developer Hosted Server now uses AT&T OAuth v4.0.

## v1.0.0-rc.10

January 22, 2015

* **Fix:** the `Phone` object will publish an error event upon invalid scenarios for a second call. The second call feature only supports simple two-call scenarios, with one call in the background (inactive) and one call in the foreground (active). Scenarios that would result in the user having one call and one conference are not supported and will generate an error.
* **Fix:** `Phone.transfer` fails with error `Transfer terminated by Network` when the transfer target is an Account ID or Virtual Number user.
* **Fix:** Corrected various typos in log statements.
* **Fix:** Moving a call fails when the call is put on hold before invoking `Phone.move`.
* **Change:** Use a short user name on `Phone` events (`event.from` and `event.to`):
  * Account ID: `sip:user@domain.com` => `user`
  * Mobile Number: `sip:1234567890@domain.com` => `1234567890`
  * Virtual User: `tel:+1234567890` => `1234567890`
* **New:** The `Phone` object will publish a `session:expired` event when the library detects that the current session has expired in the server. See the documentation for details in the payload of the event.
* **New:** Method `ATT.browser.isNetworkConnected` to check for network connectivity.

## v1.0.0-rc.9

December 31, 2014

* **Enhancement:** Added 'session:expired' event to the Phone object.
* **Enhancement:** Improved call establishment time.
* **Change:** Less verbose logging.
* **Change:** Provided full user name on event data for all phone events.

## v1.0.0-rc.7

### Features

#### Chrome v39

* Basic audio and video call management – make, receive, answer, end, mute, unmute, hold, resume, cancel, and reject calls
* Basic audio and video conferencing – create a conference, add and remove participants, hold, resume, mute, unmute, and end conference
* Advanced call management – move, transfer, add a second call, and switch between two calls
* Audio and video calls can be moved browser-to-browser
* Audio calls can be moved from a browser to an AT&T mobile phone.


#### Firefox v33

* Basic audio and video call management – make, receive, answer, end, mute, unmute, hold, resume, cancel, and reject calls
* Basic audio and video conferencing – create a conference, add and remove participants, hold, resume, mute, unmute, and end conference
