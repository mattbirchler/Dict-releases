# Privacy

Dict is built so that what you say never leaves your Mac, unless you turn
on cloud cleanup with your own OpenRouter key (described below).

## What stays on your Mac

- **Audio.** Captured from the microphone only while you hold the hotkey (or
  while hands-free mode is on), kept in memory for transcription, and
  discarded when the text is inserted. Dict never writes audio to disk.
- **Transcripts.** Produced by a speech model running on your Mac (Apple's
  built-in speech recognition, or the Parakeet model). Formatting uses the
  on-device Apple Intelligence model when it's available, unless you've
  turned on cloud cleanup.
- **History.** Recent transcriptions are stored in a local database
  (`~/Library/Application Support/Dict/History.store`) so you can copy
  them again. You choose how many to keep (or none) in History.
- **Settings and word replacements.** Stored in Dict's preferences file.
- **The field you dictate into.** When formatting runs, Dict reads the text
  already in the focused field (through the Accessibility permission) and
  hands the on-device formatter the names and terms it finds there, so they
  are spelled the way your document spells them. The text is used for that
  one dictation and never stored. It's never sent anywhere either, except
  that with cloud cleanup on, the names and terms Dict found go to
  OpenRouter along with the transcript. Turn this off in Settings > General
  if you prefer.

## The only network connections

1. **Model download.** The first time you choose the Parakeet engine, Dict
   downloads the model (about 500 MB) from `huggingface.co`, from the
   `FluidInference/parakeet-tdt-0.6b-v3-coreml` repository. The request
   carries nothing about you. The model is cached in
   `~/Library/Application Support/FluidAudio/Models`.
2. **Update check.** Dict uses Sparkle to check
   `https://raw.githubusercontent.com/mattbirchler/Dict-releases/main/appcast.xml`
   once a day, and when you choose Check for Updates. That request includes
   Dict's version and your macOS version, as every update check does, and
   nothing else. Sparkle's optional system-profile reporting is off.
3. **Anonymous usage data.** Unless you turn it off, Dict sends usage
   counts to PostHog (`us.i.posthog.com`), described below.
4. **Cloud cleanup, only if you add a key.** This is off until you paste
   your own OpenRouter API key into Settings > General > Cloud cleanup.
   Then, for each dictation that gets formatted, Dict sends the transcript
   text, the formatting instructions, and any names and terms read from the
   field you're dictating into to `openrouter.ai`, which passes them to the
   model you chose (Claude Haiku 5.5 by default). Your audio is never sent.
   Dict also asks `openrouter.ai` for your key's total spend when you click
   Check in Settings. The key is stored in your Mac's Keychain and sent only
   to OpenRouter. What OpenRouter and the model provider do with requests
   is covered by their own policies. Remove the key, or turn the toggle
   off, and these requests stop.

## Anonymous usage data

Dict sends a small set of events that show how much it gets used:
when the app launches, when a dictation finishes (its word count, its
length in seconds, the engine and the dictation style), when one fails
and why, which onboarding steps are viewed, when the Parakeet model
finishes downloading, when Buy is clicked, when a license is activated
(by link or by pasting), and when the free trial ends.

What is never sent to PostHog: your audio, your words, your history, your word
replacements, the text of the field you dictate into, and your license
key or the email address in it. Dict tells PostHog not to store your IP
address or look up your location, and doesn't create a profile for you.
Each copy of Dict has a random ID so repeat launches can be counted
together, and it isn't linked to who you are.

Turn it off in Settings > General > Share anonymous usage data. Nothing is
sent after that.

## Your license key

Buying Dict happens on a web page in your browser, handled by Stripe. The
page shows your license key, which carries the email address you bought
with and the purchase date. Dict checks the key on your Mac against a
public key built into the app. It never sends the key anywhere or asks a
server whether it's valid. The key is stored in Dict's preferences file.

There are no accounts, no crash reporting, and no other connections. The Apple speech engine runs on-device; on macOS 26 and later
it does not send audio to Apple.

## Removing everything

Quit Dict, then delete:

- `/Applications/Dict.app`
- `~/Library/Application Support/FluidAudio` (the Parakeet model)
- `~/Library/Preferences/birchtree.Dict-app.plist` (settings)
- `~/Library/Application Support/Dict` (the history database and the trial start date)
- the `birchtree.Dict-app.openrouter` item in Keychain Access, if you added
  an OpenRouter key

Dict's Microphone and Accessibility permissions can be removed in System
Settings > Privacy & Security.
