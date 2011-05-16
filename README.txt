GentleDB - The Simplest Database in the World (Bash implementation)
===================================================================

A database so simple that even my girlfriend understands it.


The idea of GentleDB is very simple
-----------------------------------

* It can store and retrieve content:
    gentledb content_id     = db + "content"
    gentledb content        = db - content_id

* It can store and retrieve a pointer ID to a content ID:
    gentledb pointer_id     = random
    gentledb db pointer_id  = content_id
    gentledb content_id     = db pointer_id

Both IDs are lower-case hexadecimal string represenations of 256-bit values.
Content IDs are the SHA-256 sum of the respective content.  Pointer IDs are the
same format, but random-generated.  Pointer IDs are equivalent to file names.


Requirements
------------

* Bash >= 3.2
* gentledb4python


Installation instructions
-------------------------

    cp gentledb.bash /usr/bin  # no need to 'chmod +x'


Tutorial
--------

    $ . gentledb.bash
    $ gentledb db = FS
    $ gentledb content_id = db + "Some content"         # store content
    $ echo $content_id                                  # SHA-256 of "Some content"
    9c6609fc5111405ea3f5bb3d1f6b5a5efd19a0cec53d85893fd96d265439cd5b
    $ gentledb db - content_id ; echo  # echo adds \n   # retrieve content
    Some content
    $ gentledb pointer_id = random                      # random value to keep
    $ gentledb db pointer_id = content_id               # store pointer to content
    $ gentledb retrieved_content_id = db pointer_id     # retrieve content from pointer
    $ gentledb db pointer_id ; echo $retrieved_content_id
    9c6609fc5111405ea3f5bb3d1f6b5a5efd19a0cec53d85893fd96d265439cd5b
    9c6609fc5111405ea3f5bb3d1f6b5a5efd19a0cec53d85893fd96d265439cd5b


GentleDB for Bash also supports
-------------------------------

* Standard input and standard output:
    gentledb content_id = db + < datafile
    # Pipes create subshells, and thus can't be used with variable assignments:
    # produce_content | gentledb cid = db +   # will fail explicitly - instead do:
    produce_content | gentledb db + > content_id_file  # or:
    content_id="$(produce_content | gentledb db +)"    # or:
    cnt="$(produce_content ; echo x)" && cnt="${cnt%x}" && gentledb cid = db + "$cnt"

    gentledb db - content_id > datafile
    gentledb db - content_id | consume_content


Copyright statement
-------------------

Copyright (C) 2011  Felix Rabe (www.felixrabe.net)

GentleDB is free software; you can redistribute it and/or
modify it under the terms of the GNU Lesser General Public
License as published by the Free Software Foundation; either
version 2.1 of the License, or (at your option) any later version.

GentleDB is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the GNU
Lesser General Public License for more details.

You should have received a copy of the GNU Lesser General Public
License along with GentleDB.  If not, see <http://www.gnu.org/licenses/>.


The file lgpl-2.1.txt, included in the library's distribution, contains the
license text.
