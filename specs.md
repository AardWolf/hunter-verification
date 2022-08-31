# Purpose

This bot's purpose is to allow for an automated or semi-automated verification process between Discord and the Hitgrab game MouseHunt. It assumes the bot will have access to the semi-public hunter's corkboard which requires the bot be logged in to the game. This linkage will result in a role being assigned and the relationship being queryable in different ways depending on the Discord server roles of the user making the query. All queries should be logged and observable or have cooldowns to prevent abuse.

# Principles

A major goal here is to keep the actual list/database private. This includes any backups of the database. The end product must also prevent being able to easily find a discord id based on a hunter id in general while allowing the activity in a privileged environment. It should allow people to verify accounts are linked but leak as little data as possible when doing so.

# Workflows and Commands

## Configure

`/hv-config` will allow some configuration options with simple permission requirements such as Administer Server. Things that can be configured (potentially):
- admin role: Use this role instead of Administer Server for admin-level commands such as configure.
- mod role: Use this role instead of locking to a channel for moderation commands
- mod channel: Use this channel instead of a role for moderation commands
- enable: Turn on or off commands
- check count: The number of times to check if a user puts the phrase on the board before giving up (and telling the user)
- check time: The time to wait between check cycles
- cooldown: The time between certain commands being run by a user
- logchannel: The channel to log into. This may be split into two channels for high-level and low-level messages.

## Register

`/register hunterid:<hunterid>` (perhaps) begins the registration process. The bot would provide a unique phrase for the hunter. This does not have to be wholly unique but collisions should be infrequent. The hunter would write this phrase on their corkboard. The bot will periodically poll users who are in this phase of the workflow and look for "recent" corkboard messages that contain the phrase. This can be an exact match or close to it. Upon seeing the phrase the user should receive the verified role and a DM to tell them they have received it. Failure to DM should be logged but not considered a complete failure.

If registration is pending and command is used again, allow to provide a different hunter id; optionally provide a new phrase.

## De-Register

`/deregister`: Optionally require confirmation. Removes the user from the registry. Should be a cooldown for hunter id to be re-registered or otherwise log that the deregistration happened. If registration was still pending, stop the registration.

## Check

`/hv-check hunterid:<hunterid> discordid:discordid`: Check the registration of a discord user. May allow other methods than just discordid. May allow snuid lookup. Only returns a yes/no answer. Logs executions somewhere. Used to confirm a person is who they say they are. If used on oneself, provide linkage information (pending, when successful). The response should be ephemeral.

## Lookup - Mod+

`/hv-lookup [hunterid|discordid]`: Looks up either the hunter id or discord id given the other parameter. Should be confined to a channel or role. Should also provide a message if the linkage is still pending.

## Cleanup - Admin

`/hv-cleanup`: Cleans up users no longer in the server. Should log all freed hunter ids/user ids.

# Phrases

Phrases should be simple and could be generated from three arrays. For example, color + animal + noun will lead to things like "purple monkey dishwasher". We could use in-game terms or whatever seems fun and still have enough options to be fairly random. 

# Other Considerations

This bot should only be in one server per instance. Cross-server registration would require additional "tables" while a single server can use a single json "database" (memory concerns!).
