---
assurance:
  id: t-2
  base: sha256:c696667de479cc8be8e0c1668db1f58e0c9643312eced69df369f6d497637849
---
# Verify supported release formats and publication languages for self-publishing

> Prove that the release flow exposes the supported publication formats, the simultaneous paperback-and-eBook release option, and the supported publication languages.

## Step 1

Open https://notionpress.com/ and navigate to the self-publishing page that describes the publishing workflow, supported formats, and supported languages.

## Step 2

Scroll to the publishing workflow section until the "release simultaneously as paperback and eBook" statement is on screen, then wait 3 seconds for the section to finish rendering.

## Step 3 @verifies ac-2

Assert the page states that a book can be released simultaneously as paperback and eBook.

## Step 4 @verifies ac-3

Assert the same "Publish your book in both eBook and paperback formats" statement names Paperback and eBook as the two release formats.

## Step 5

Scroll to the language support section until the list of supported languages is on screen, then wait 3 seconds for the section to finish rendering.

## Step 6 @verifies ac-4

Assert English, Hindi, Tamil, Bengali, Marathi, Malayalam, Gujarati, and Kannada are all listed as supported publication languages.
