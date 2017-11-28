# Testing

## Automated Tests: Requirements

 * Working CiviCRM installation
 * PHPUnit (https://phpunit.de/)
 * cv (https://github.com/civicrm/cv)

## Automated Tests: End to end

This extension includes a handful of end-to-end API tests.  These tests
verify that the Mosaico data can be read and written when installed on a
CiviCRM site.

```
phpunit4 --group e2e
```

## Manual Tests: Save/Load Mailing

1. Create a mailing
    1. Navigate to "Mailings => New Mailing" (`civicrm/a/#/mailing/new` or `civicrm/a/#/mailing/new/mosaico`)
    2. _Observe_: Redirect to `civicrm/a/#/mailing/123` and open with Mosaico layout
    3. Enter a mailing name, etc. (Make a mental note of the name.)
    4. Under "Design", choose a template.
    5. _Observe_: A full-screen dialog opens with Mosaico.
    6. Create a block. Edit some text. (Make a mental note of the content.)
    7. Save.
2. Immediately re-edit
    1. Under "Design", open the template again.
    2. _Observe_: A full-screen dialog opens with Mosaico. It restores the content from before.
    3. Save or close
3. Try a full reload
    1. Navigate to "Mailings => Draft and Unscheduled"
    2. Find your mailing. Click "Continue".
    3. Under "Design", open the template again.
    4. _Observe_: A full-screen dialog opens with Mosaico. It restores the content from before.

## Manual Tests: Tokens

This extension defines a TinyMCE plugin called "civicrmtoken".  To test this
plugin, create a mailing with a block of content. Then try each of the following:

1. Edit a paragraph. In the toolbar, click the token dropdown and see a hotlist of 3-5 tokens. Pick one. Observe the new token in the paragraph.
2. Edit a paragraph. In the toolbar, click the token icon and see a dialog. Pick one. Observe the new token in the paragraph.
3. Edit a paragraph. Press Ctrl-Shift-T and see a dialog. Enter a filter and pick a token using the keyboard. Observe the new token in the paragraph.
4. Edit a heading or button. Ensure that the the token icon/dropdown/hotkey work as expected. Observe the new token in the heading or button.

## Manual Tests: Images and Links

Mosaico handles a few different kinds of images and links. To test these, create a
mailing with content:

 * (A1) Add a block which supports an image. Upload an image.
 * (A2) Add a block which supports an image. Re-use the previously uploaded image.
 * (A3) Add a footer block which uses the built-in Twitter/Facebook icons.
 * (A4) In one of the blocks, add a hyperlink to an external page (eg `https://google.com`).
 * (A5) In one of the blocks, add a hyperlink to an internal page (eg `http://dmaster.l/user`).
 * (A6) In one of the blocks, add a hyperlink to using a token (eg `{action.forward}`)

Now, we're going test that content appears correctly in several scenarios.  Each scenario references the "Message Evaluation Procedure" (defined further down):

 * (B1) Using the "Test", open the "Preview" in HTML. Perform the "Message Evaluation Procedure" (in the browser).
 * (B2) Using the "Test", send a message to an email address. Perform the "Message Evaluation Procedure" (in the email).
 * (B3) Finalize and submit the mailing. Trigger cron (eg `cv api -U admin job.process_mailing`). Perform the "Message Evaluation Procedure" (in the email).
 * (B4) In the email, click the link to "View in Browser". Perform the "Message Evaluation Procedure" (in the browser).

The "Message Evaluation Procedure" is:

 * (C1) Check the the uploaded image. The image should appear and use an absolute URL.
 * (C2) Check the the re-used uploaded image. The image should appear and use an absolute URL.
 * (C3) Check the the Twitter/Facebook icons. The image should appear and use an absolute URL.
 * (C4) Check the hyperlink to the external page (eg `https://google.com`). The link should be absolute. (For a finalized mailing B3/B4, the link should work.)
 * (C5) Check the hyperlink to the internal page (eg `http://dmaster.l/user`). The link should be absolute. (For a finalized mailing B3/B4, the link should work.)
 * (C6) Check the hyperlink created via token. The link should be absolute. (For finalized mailing B3/B4, the link should work.)

## Manual Tests: Save/Load Template

1. Create a new template
    1. Navigate to "Mailings => Mosaico Templates"
    2. _Observe_: There is a section for "Create new template from...".
    3. _Observe_: There *may be* a section "Configured templates" if some templates exist.
    4. Under "Create new template from...", select one of the base templates like "Versafix 1"
    5. Enter a template name. (Make a mental note of the name.)
    6. _Observe_: A full-screen dialog opens with Mosaico.
    7. Create a block. Edit some text. (Make a mental note of the content.)
    8. Save.
2. Immediately re-edit
    1. Under "Configured templates", find your template and click the thumbnail.
    2. _Observe_: A full-screen dialog opens with Mosaico. It restores the content from before.
    3. Save
3. Try a full reload
    1. Go to any other page (such as the dashboard).
    2. Navigate to "Mailings => Mosaico Templates"
    3. Under "Configured templates", find your template and click the thumbnail.
    4. _Observe_: A full-screen dialog opens with Mosaico. It restores the content from before.
4. Copy a template
    1. Under "Configured templates", find your template and click the copy icon.
    2. Enter a new template name. (Make a mental note of the name.)
    3.  _Observe_: A full-screen dialog opens with Mosaico. It restores the content from before.
    4. Create another block.  Edit some text.
    5. Save
    6. Open both the old and new templates. Check that they have the right content.
5. Delete the copied template
    1. Under "Configured templates", find your new copied template and click the red X.
    2. _Observe_: The template goes away.
6. Rename a template
    1. Under "Configured templates", find your template and click the wrench icon.
    2. Enter a new template name.
    3. _Observe_: The name updates.
