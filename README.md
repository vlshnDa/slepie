# slepie
Use system announcements
– Instead of using AVSpeechSynthesizer directly, call:

swift
Copy
UIAccessibility.post(notification: .announcement, argument: text)
This ensures VoiceOver’s user settings (voice, rate, pitch) are applied.

Check VoiceOver status
– Before speaking, do:

swift
Copy
if UIAccessibility.isVoiceOverRunning { … }
and choose between post(notification:) and AVSpeechSynthesizer accordingly.

Configure your audio session
– For any AVFoundation speech, set your session so it doesn’t interrupt VoiceOver:

swift
Copy
try? AVAudioSession.sharedInstance().setCategory(.ambient,
      options: [.mixWithOthers, .duckOthers])
try? AVAudioSession.sharedInstance().setActive(true)
Properly label UI elements
– Always provide accessibilityLabel, and when needed accessibilityHint and accessibilityTraits for interactive controls.
– Mark purely decorative images with accessibilityHidden(true).

Don’t use images for text
– Always present text via Text, Label, Button, etc., never as a bitmap.

Manage focus order after animations or layout changes
– When new views appear or screens change, post a layout change notification:

swift
Copy
UIAccessibility.post(notification: .layoutChanged, argument: elementToFocus)
Avoid repeated announcements
– Track whether you’ve already spoken a given message (e.g. with a hasAnnounced flag) to prevent duplicates.

Test with VoiceOver and Accessibility Inspector
– Exercise every screen and control on-device and in the Simulator, and validate metadata with Xcode’s Accessibility Inspector.

Support localization
– Move all spoken strings and accessibilityLabels into .strings files and provide translations for each supported language.

Respect user settings
– Always honor the user’s chosen speech rate, language, and pitch from the system VoiceOver settings.








