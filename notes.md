Random stuff to keep in mind.

# Verification, Hunter IDs, and SNUIDs

When looking at someone else's profile page by hunter-id you can get their snuid from userInteractionButtonsView full_buttons data-recipient-snuid property. Then use that to make sure that snuid wrote that message on that corkboard. If looking at the profile by snuid you can get the hunterid from the hunterInfoView-hunterId-idText or messageBoardView data-owner-unique-id property.

Messages on the corkboard use the snuid to identify who sent them. messageBoardView-message-name using the href or onClick property and a regex. messageBoardView-message-body .text or similar will be the path to get to the message body. messageBoardView-message-submitted has a datetime string ('Month day, year hh:mm' with am/pm at the end stuck to the minutes).
