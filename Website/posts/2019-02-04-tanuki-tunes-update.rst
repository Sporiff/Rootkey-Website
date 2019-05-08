===================
Tanuki Tunes Update
===================

.. post:: Feb 04, 2019
   :author: Sporiff
   :tags: funkwhale, federation, tech, mastodon
   :category: Tech
   :location: Exeter
   :language: en
   :redirect: 2019-02-04-tanuki-tunes-update

Another Funkwhale Update
------------------------

After a little bit of frustration on my side due to not
following the directions properly, `Tanuki Tunes <https://tanukitunes.com/about>`_
is now running `Funkwhale <https://funkwhale.audio>`_ 0.18.1.
The changes in this version will be mostly invisible to end-users,
but include important bugfixes such as one that prevented the
editing of tracks in the Django admin panel. This update has also
made me aware that the site was previously running an out-of-date
version of Postgresql. This has now been rectified, so the site
is completely up-to-date with suggested specs.

A Sad Update
------------

As many of you were probably aware, this weekend was the weekend of FOSDEM
out in Brussels. Yours truly was supposed to be in attendance, however
a drizzling of snow shut down all local airports and forced me to give up
trying to leave the country after trying for more than 24 hours. I'm going
to be catching up on the videos from the conference over the next few days and
look forward to attending next year (hopefully!)

Other Stuff
-----------

This past month has been steady for both `Baku Social <https://bakusocial.com/about>`_
and `Tanuki Tunes <https://tanukitunes.com/about>`_, with the only downtime
being due to the upgrade to the latter. This affected Baku Social due to my
lack of experience working with Postgresql and Docker, but it's fair to say
I've learned a lot from the process. Hopefully this will go smoother in future
and the two sites should be spun up and down separately from one another at all times
as is the point of running them from Docker.

I am still awaiting the final sketches for Baku Social's new mascot. I've been
working with the artist to make something that reflects both Mastodon's fun,
cartoonish vibe and the mythological presence of the Baku. It should be great once
it's done!

`Tsukumogami <https://bakusocial.com/@tsukumogami>` and `FolkloreThursdayBot <https://bakusocial.com/@folklorethursdaybot>`_
have been working well this week with the exception of a double-posting bug with
FTBot which I need to look into. Once that's solved, I'll be looking in to other
content to make bots for. Any suggestions are welcome, just PM me on `Mastodon <https://bakusocial.com/@sporiff>`_.

Lastly, I've updated the theme for `Rootkey <https://rootkey.co.uk>`_ just for a change
of pace. I've also made a dedicated `subdirectory for any music I post <https://rootkey.co.uk/music/>`_, all of which
can be streamed directly on the site from `Tanuki Tunes <https://tanukitunes.com/about>`_
thanks to a new embed feature included in version 0.18 of Funkwhale.

That's all for now, folks!
