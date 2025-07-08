
## XII. Examples, help

For additional help
 please visit the RSL thread on MonsterNet or join #rsl on IRC
 (irc.droidarena.com). If you would like to have a more detailed explanation
 about a command, event or function, please send an email to [rsl@droidarena.net](#mailto:rsl@droidarena.net). This is a
 list where we will try to answer any questions you might have about RSL. We only
 ask you to contact us only when you have checked all the other alternatives and
 you can not find help.

- [LMSG](#lmsg), [SMSG](#smsg) example

```rsl
[INIT]
	; I could save a few messages (although we only have 10
	; &ms variables to save to, at least we can change
	; these any time)
	LMSG &ms0 Meet me at our secret Position!

[MSGR]
	; Seems like I got a message!
	; I save the sender here, so I can answer later on
	COPY #id &id0
	; I check if the message is what I am expecting it to be
	; (if it's the one in &ms0)
	; if yes, i jump to MEET_NOW, else execution should
	; continue in OTHER_MESSAGE
	ISEQ #ms &ms0 MEET_NOW OTHER_MESSAGE

[MEET_NOW]
	; Lets say the secret Position is at 23:48
	CPOS 23 48
	GOTO #ps
	; I could answer as well, the sender was saved into &id0
	SMSG &id0 Roger! I'm going there.

[OTHER_MESSAGE]
	; This is a message that I was not prepared for.
	; We could check if it's an ally or an enemy who sent
	; this message, and respond according to that, but for now
	; we could just say "Huh???"
	SMSG &id0 Huh???
```

- [FRND](#frnd) example

Using [FRND](#frnd) in the previous example, `[OTHER_MESSAGE]` could be:

```rsl
[OTHER_MESSAGE]
	FRND &id0 MSG_FROM_FRIEND MSG_FROM_ENEMY [MSG_FROM_FRIEND]
	; the message arrived from a friendly unit
	SMSG &id0 I don't understand, mate.

[MSG_FROM_ENEMY]
	; the message arrived from an enemy unit
	SMSG &id0 Threats? Prepare to die!
	; and then we could check if this enemy is visible, etc..
```

- [DOLL](#doll), [DPIK](#dpik), [DLOS](#dlos) and <#dl> example

[DOLL](#doll) is executed when the robot
 sees the doll. Be careful, as it is executed in every tick as long as the doll
 is visible to the robot. The most obvious thing to do when we see the doll could
 be getting it, although in some situations you would probably want to call
 someone else pick it up in your team. Let's see two simple examples:

```rsl
[DOLL]
	; we see the doll finally! #ps - the Position of the doll
	; is passed, so we can use it for a GOTO command
	GOTO #ps
	; However, we might want to do something else when we see it.
	; Lets assume we want to call "Webbed" over to get the doll.
	; Since we can not pass the Position of the doll, the best
	; would be if he would come to where we stand at the moment.
	; If we agreed with him, that he will come to our Position
	; whenever we send him the message "I see the doll!", it will
	; be pretty easy for him to do so (since he knows we sent the
	; message, and he can get our Position by using GPOS).
	; We could wait as long as he gets there (we see him) and then
	; we could follow him. (Be careful, if he'd be destroyed on the
	; way there, we could wait for him for a long time - I leave
	; it to you to solve that problem).
```

```rsl
[INIT]
	; We set &id to Webbed's RobotId to be able to identify him
	; later during the game
	CNAM Webbed
	COPY #id &id0
	; After this we can just roam around and hope to find the
	; doll. Let's trigger [POSI] by going to our own Position
	; stored in #mp.
	GOTO #mp

[DOLL]
	; We send Webbed the message "I see the doll!"
	SMSG &id0 I see the doll!
	; and check if he's here yet. Since [DOLL] executes
	; each tick as long as the doll is visible, it is just the
	; right place to check. It doesn't matter if we send the
	; message several times, that should be Webbed's problem ;)
	VISI &id0 WEBBED_IS_HERE WEBBED_IS_NOT_HERE_YET

[WEBBED_IS_HERE]
	; Seems like he made it. Let's follow him then.
	; Don't forget that
	FOLL &id0

[WEBBED_IS_NOT_HERE_YET]
	; He's not here yet, we should probably wait a little
	; longer. If we use WAIT, the bot will not move
	WAIT 10

[POSI]
	; To search for the doll, we will need to use POSI, but
	; since we don't necessarily want it to look for random
	; Positions to move to - whether we see Webbed or not,
	; again, we can use VISI to do the trick.
	; If we can't see Webbed, then we could go to a random
	; spot, else we could follow him.
	VISI &id0 WEBBED_IS_HERE GOTO_RANDOM_Position

[GOTO_RANDOM_Position]
	GRPS
	GOTO #ps
```

Thank you for reading through this reference. If you have a
 question or you have a good example for us to put in this document, please let
 us know! Email us: [rsl@droidarena.net](#mailto:rsl@droidarena.net).
