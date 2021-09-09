# key-gnu
key gnu

From wk at gnupg.org  Sun Aug  1 10:40:55 2021
From: wk at gnupg.org (Werner Koch)
Date: Sun, 01 Aug 2021 10:40:55 +0200
Subject: 2.3.1: compilation result without dirmngr (due to --disable-ldap?)
In-Reply-To: <20210728150419.DEIXG%steffen@sdaoden.eu> (Steffen Nurpmeso's
 message of "Wed, 28 Jul 2021 17:04:19 +0200")
References: <20210728150419.DEIXG%steffen@sdaoden.eu>
Message-ID: <87v94p5zhk.fsf@wheatstone.g10code.de>

On Wed, 28 Jul 2021 17:04, Steffen Nurpmeso said:

> and the compilation does not include dirmngr, making the entire
> installation useless.  (I personally still use gpg (GnuPG) 1.4.23,

I just tried with --disable-ldap and --disable-nls and can't see a
problem.  it the current master version though.

> i have not looked at the protocol, but sigh that not
> a standardized checksum over the email address was chosen, like

SHA-1 is a very standard algorithm and fully sufficient for the purpose
here; i.e. mapping a string to a fixed length identifier.  SHA-1 is
anyway a required part of OpenPGP and there have been no security
weaknesses found its use case as fingerprint algorithms.


Shalom-Salam,

   Werner

-- 
Die Gedanken sind frei.  Ausnahmen regelt ein Bundesgesetz.
-------------- next part --------------
A non-text attachment was scrubbed...
Name: signature.asc
Type: application/pgp-signature
Size: 227 bytes
Desc: not available
URL: <https://lists.gnupg.org/pipermail/gnupg-devel/attachments/20210801/7b113c40/attachment-0001.sig>

From steffen at sdaoden.eu  Mon Aug  2 15:47:03 2021
From: steffen at sdaoden.eu (Steffen Nurpmeso)
Date: Mon, 02 Aug 2021 15:47:03 +0200
Subject: 2.3.1: compilation result without dirmngr (due to --disable-ldap?)
In-Reply-To: <87v94p5zhk.fsf@wheatstone.g10code.de>
References: <20210728150419.DEIXG%steffen@sdaoden.eu>
 <87v94p5zhk.fsf@wheatstone.g10code.de>
Message-ID: <20210802134703.TlVIZ%steffen@sdaoden.eu>

Werner Koch wrote in
 <87v94p5zhk.fsf at wheatstone.g10code.de>:
 |On Wed, 28 Jul 2021 17:04, Steffen Nurpmeso said:
 |
 |> and the compilation does not include dirmngr, making the entire
 |> installation useless.  (I personally still use gpg (GnuPG) 1.4.23,
 |
 |I just tried with --disable-ldap and --disable-nls and can't see a
 |problem.  it the current master version though.

Fine this is fixed.

 |> i have not looked at the protocol, but sigh that not
 |> a standardized checksum over the email address was chosen, like
 |
 |SHA-1 is a very standard algorithm and fully sufficient for the purpose
 |here; i.e. mapping a string to a fixed length identifier.  SHA-1 is
 |anyway a required part of OpenPGP and there have been no security
 |weaknesses found its use case as fingerprint algorithms.

Yes, no, my problem is about the the special z-base-32 step, for
which there is no tool around by default.  But i personally still
struggle with the base64 that SSH now uses for fingerprinting,
i find this very hard.  Yes i had seen discussion in the PGP IETF
list about such base'ing, but i _personally_ cannot grasp
z5fuz1m868tz5eeq3y86cnomqztbbyjd.  Now that i have RFC 6189
i could of course take the algorithm of section 5.1.6 and
implement it.  You know.  It is more like .. i did not understand
why so complicated as that is nowhere human anyway, is it?  Well,
unless you plan to use this way of hashing as a default in
a future GnuPG version of course.  (I personally would very much
favour these nice groups of four hexdecimal bytes, as can be
produced with --fingerprint (in 1.4.*), even though it gets very
lengthy with SHA-256 or longer, but people only look at the tail
and the front, and maybe snippets in the middle, i think that was
talked about in the IETF group like this, no? .. i can confirm.
Ok, maybe if grouped by four it would work out anyway, looking at
it.  But nonetheless.  If grouped by four i would _assume_ that
lower/upper would even help differentiating, ie, base64 because it
is in use quite often, with OpenSSH even in user view.  You know,
just doing echo BLA|sha1sum|base64 if you are on a Unix.
Whatever.  Greetings to NRW!)

Ciao,

--steffen
|
|Der Kragenbaer,                The moon bear,
|der holt sich munter           he cheerfully and one by one
|einen nach dem anderen runter  wa.ks himself off
|(By Robert Gernhardt)


From gnupg-devel at spodhuis.org  Tue Aug  3 22:53:37 2021
From: gnupg-devel at spodhuis.org (Phil Pennock)
Date: Tue, 3 Aug 2021 16:53:37 -0400
Subject: Update keys.gnupg.net?
In-Reply-To: <87pmuy7qf0.fsf@wheatstone.g10code.de>
References: <7201645.5QdGYcIf76@daneel> <875yx5c6kp.fsf@wheatstone.g10code.de>
 <877dhg3ovc.fsf@latte.josefsson.org>
 <878s1sb0wh.fsf_-_@wheatstone.g10code.de>
 <8735s0glt6.fsf@latte.josefsson.org>
 <87h7gfabyf.fsf@wheatstone.g10code.de>
 <87eebig2br.fsf@latte.josefsson.org>
 <871r7ia2cj.fsf@wheatstone.g10code.de>
 <YQLcyDcLK2TxQ7PO@hill.local>
 <87pmuy7qf0.fsf@wheatstone.g10code.de>
Message-ID: <YQms0XvNCiSCgAFm@fullerene.field.pennock-tech.net>

On 2021-07-31 at 12:01 +0200, Werner Koch via Gnupg-devel wrote:
> On Thu, 29 Jul 2021 12:52, Phil Pennock said:
> 
> > I have both an RSA key and an Ed25519 key.  Every time I think I can
> > just use the Ed25519 key, I encounter a new system where that assumption
> > fails, so even for $work I ended up having to create an RSA key too.
> 
> So what you are saying is that there are implementations with Web Key
> Directory support but no support for Ed25519?

No.  This was the point of the lead-in paragraph in what I wrote:

} or
} should it also be useful when people are trying to migrate to an
} algorithm not yet implemented in GnuPG, years from now when WKD is the
} ubiquitous infrastructure for pulling keys?

If WKD is assuming that "new enough to support WKD" means "new enough to
support the latest key algorithm which people want to use", then that
assumption will age poorly.

-Phil


From wk at gnupg.org  Wed Aug  4 12:59:16 2021
From: wk at gnupg.org (Werner Koch)
Date: Wed, 04 Aug 2021 12:59:16 +0200
Subject: Update keys.gnupg.net?
In-Reply-To: <YQms0XvNCiSCgAFm@fullerene.field.pennock-tech.net> (Phil Pennock
 via Gnupg-devel's message of "Tue, 3 Aug 2021 16:53:37 -0400")
References: <7201645.5QdGYcIf76@daneel> <875yx5c6kp.fsf@wheatstone.g10code.de>
 <877dhg3ovc.fsf@latte.josefsson.org>
 <878s1sb0wh.fsf_-_@wheatstone.g10code.de>
 <8735s0glt6.fsf@latte.josefsson.org>
 <87h7gfabyf.fsf@wheatstone.g10code.de>
 <87eebig2br.fsf@latte.josefsson.org>
 <871r7ia2cj.fsf@wheatstone.g10code.de> <YQLcyDcLK2TxQ7PO@hill.local>
 <87pmuy7qf0.fsf@wheatstone.g10code.de>
 <YQms0XvNCiSCgAFm@fullerene.field.pennock-tech.net>
Message-ID: <8735rp4gsb.fsf@wheatstone.g10code.de>

On Tue,  3 Aug 2021 16:53, Phil Pennock said:

> If WKD is assuming that "new enough to support WKD" means "new enough to
> support the latest key algorithm which people want to use", then that
> assumption will age poorly.

Right, however for ed25519 it helds true.  We had the same situation
back when we introduced the MDC - we could assume that support for AES
also meant that MDC is supported.  In fact MDC was born at the second
AES conference.


Shalom-Salam,

   Werner

-- 
Die Gedanken sind frei.  Ausnahmen regelt ein Bundesgesetz.
-------------- next part --------------
A non-text attachment was scrubbed...
Name: signature.asc
Type: application/pgp-signature
Size: 227 bytes
Desc: not available
URL: <https://lists.gnupg.org/pipermail/gnupg-devel/attachments/20210804/b71b6bc9/attachment.sig>

From wk at gnupg.org  Wed Aug  4 12:56:01 2021
From: wk at gnupg.org (Werner Koch)
Date: Wed, 04 Aug 2021 12:56:01 +0200
Subject: 2.3.1: compilation result without dirmngr (due to --disable-ldap?)
In-Reply-To: <20210802134703.TlVIZ%steffen@sdaoden.eu> (Steffen Nurpmeso's
 message of "Mon, 02 Aug 2021 15:47:03 +0200")
References: <20210728150419.DEIXG%steffen@sdaoden.eu>
 <87v94p5zhk.fsf@wheatstone.g10code.de>
 <20210802134703.TlVIZ%steffen@sdaoden.eu>
Message-ID: <877dh14gxq.fsf@wheatstone.g10code.de>

On Mon,  2 Aug 2021 15:47, Steffen Nurpmeso said:

> Yes, no, my problem is about the the special z-base-32 step, for
> which there is no tool around by default.  But i personally still

Right, same for the very new, silly, and one-usecase base-45.
(gnupg/common/t-zb32.c is not installed but might be helpful)

> struggle with the base64 that SSH now uses for fingerprinting,
> i find this very hard.  Yes i had seen discussion in the PGP IETF

Me too.  That has been the worsed decision they ever made.  Abbreviated
SHA256 would have be totally sufficient and gians better security due to
a better UX.


Salam-Shalom,

   Werner


-- 
Die Gedanken sind frei.  Ausnahmen regelt ein Bundesgesetz.
-------------- next part --------------
A non-text attachment was scrubbed...
Name: signature.asc
Type: application/pgp-signature
Size: 227 bytes
Desc: not available
URL: <https://lists.gnupg.org/pipermail/gnupg-devel/attachments/20210804/d82fd84c/attachment.sig>

From wk at gnupg.org  Tue Aug 24 19:51:51 2021
From: wk at gnupg.org (Werner Koch)
Date: Tue, 24 Aug 2021 19:51:51 +0200
Subject: [Announce] GnuPG 2.3.2 released
Message-ID: <87eeaispc8.fsf@wheatstone.g10code.de>

Hello!

We are pleased to announce the availability of a new GnuPG release:
version 2.3.2.  This is the third release in the new 2.3 series which
fixes a couple of bugs and adds a few new things.  See below for
details.


What is GnuPG
=============

The GNU Privacy Guard (GnuPG, GPG) is a complete and free implementation
of the OpenPGP and S/MIME standards.

GnuPG allows to encrypt and sign data and communication, features a
versatile key management system as well as access modules for public key
directories.  GnuPG itself is a command line tool with features for easy
integration with other applications.  The separate library GPGME provides
a uniform API to use the GnuPG engine by software written in common
programming languages.  A wealth of frontend applications and libraries
making use of GnuPG are available.  As an universal crypto engine GnuPG
provides support for S/MIME and Secure Shell in addition to OpenPGP.

GnuPG is Free Software (meaning that it respects your freedom).  It can
be freely used, modified and distributed under the terms of the GNU
General Public License.

Three different series of GnuPG are actively maintained:

- Version 2.3 is the latest development series with a lot of new
  features.  This announcement is about the latest release of this
  series.

- Version 2.2 is our LTS (long term support) version and guaranteed to
  be maintained at least until the end of 2024.
  See https://gnupg.org/download/index.html#end-of-life

- Version 1.4 is maintained to allow decryption of very old data which
  is, for security reasons, not anymore possible with other GnuPG
  versions.


Noteworthy changes in version 2.3.2
===================================

  * gpg: Allow fingerprint based lookup with --locate-external-key.
    [ec36eca08c]

  * gpg: Allow decryption w/o public key but with correct card
    inserted.  [50293ec2eb]

  * gpg: Auto import keys specified with --trusted-keys.  [100037ac0f]

  * gpg: Do not use import-clean for LDAP keyserver imports.  [#5387]

  * gpg: Fix mailbox based search via AKL keyserver method.  [4fcfac6feb]

  * gpg: Fix memory corruption with --clearsign introduced with 2.3.1.
    [#5430]

  * gpg: Use a more descriptive prompt for symmetric decryption.
    [6dfae2f402]

  * gpg: Improve speed of secret key listing.  [40da61b89b]

  * gpg: Support keygrip search with traditional keyring.  [#5469]

  * gpg: Let --fetch-key return an exit code on failure.  [#5376]

  * gpg: Emit the NO_SECKEY status again for decryption.  [#5562]

  * gpgsm: Support decryption of password based encryption (pwri).
    [eeb65d3bbd]

  * gpgsm: Support AES-GCM decryption.  [4980fb3c6d]

  * gpgsm: Let --dump-cert --show-cert also print an OpenPGP
    fingerprint.  [52bbdc731f]

  * gpgsm: Fix finding of issuer in use-keyboxd mode.  [6b76693ff5]

  * gpgsm: New option --ldapserver as an alias for --keyserver.
    [89df86157e]

  * agent: Use SHA-256 for SSH fingerprint by default.  [#5434]

  * agent: Fix calling handle_pincache_put.  [#5436]

  * agent: Fix importing protected secret key.  [#5122]

  * agent: Fix a regression in agent_get_shadow_info_type.  [#5393]

  * agent: Add translatable text for Caps Lock hint.  [#4950]

  * agent: New option --pinentry-formatted-passphrase.  [#5517]

  * agent: Add checkpin inquiry for pinentry.  [#5517,#5532]

  * agent: New option --check-sym-passphrase-pattern.  [#5517]

  * agent: Use the sysconfdir for a pattern file.

  * agent: Make QT_QPA_PLATFORMTHEME=qt5ct work for the pinentry.
    [1305baf099]

  * dirmngr: LDAP search by a mailbox now ignores revoked keys.
    [1406f551f1]

  * dirmngr: For KS_SEARCH return the fingerprint also with LDAP.
    [#5441]

  * dirmngr: Allow for non-URL specified ldap keyservers.  [#5405,#5452]

  * dirmngr: New option --ldapserver.  [52cf32ce2f]

  * dirmngr: Fix regression in KS_GET for mail address pattern.
    [#5497]

  * card: New option --shadow for the list command.  [2fce99d73a]

  * tests: Make sure the built keyboxd is used.  [#5406]

  * scd: Fix computing shared secrets for 512 bit curves.
    [9e24f2a45c]

  * scd: Fix unblock PIN by a Reset Code with KDF.  [#5413]

  * scd: Fix PC/SC removed card problem.  [8d81fd7c01]

  * scd: Recover the partial match for PORTSTR for PC/SC.
   [53bdc6288f]

  * scd: Make sure to release the PC/SC context.  [#5416]

  * scd: Fix zero-byte handling in ECC.  [#5163]

  * scd: Fix serial number detection for Yubikey 5.  [#5442]

  * scd: Add basic support for AET JCOP cards.  [544ec7872a]

  * scd: Detect external interference when --pcsc-shared is in use.
    [#5484]

  * scd: Fix access to the list of cards.  [#5524]

  * gpgconf: Do not list a disabled tpm2d.  [#5408]

  * gpgconf: Make runtime changes with different homedir work.
    [31c0aa2ff3]

  * keyboxd: Fix searching for exact mail adddress.  [f79e9540ca]

  * keyboxd: Fix searching with multiple patterns.  [101ba4f18a]

  * gpgtar: Fix file size computation under Windows.  [14e36bdbe1]

  * tools: Extend gpg-check-pattern.  [73c03e0232]

  * wkd: Fix client issue with leading or trailing spaces in
    user-ids.  [b4345f7521]

  * Under Windows add a fallback in case the console can't cope with
    Unicode.  [#5491]

  * Under Windows use LOCAL_APPDATA for the socket directory.  [#5537]

  * Pass XDG_SESSION_TYPE and QT_QPA_PLATFORM envvars to Pinentry.
    [#3659]

  * Change the default keyserver to keyserver.ubuntu.com.  This is a
    temporary change due to the shutdown of the SKS keyserver pools.
    [55b5928099]

  Release-info: https://dev.gnupg.org/T5405


Getting the Software
====================

Please follow the instructions found at <https://gnupg.org/download/> or
read on:

GnuPG may be downloaded from one of the GnuPG mirror sites or direct
from its primary FTP server.  The list of mirrors can be found at
<https://gnupg.org/download/mirrors.html>.  Note that GnuPG is not
available at ftp.gnu.org.

The GnuPG source code compressed using BZIP2 and its OpenPGP signature
are available here:

 https://gnupg.org/ftp/gcrypt/gnupg/gnupg-2.3.2.tar.bz2 (7411k)
 https://gnupg.org/ftp/gcrypt/gnupg/gnupg-2.3.2.tar.bz2.sig

An installer for Windows without any graphical frontend except for a
very minimal Pinentry tool is available here:

 https://gnupg.org/ftp/gcrypt/binary/gnupg-w32-2.3.2_20210824.exe (4586k)
 https://gnupg.org/ftp/gcrypt/binary/gnupg-w32-2.3.2_20210824.exe.sig

The source used to build the Windows installer can be found in the same
directory with a ".tar.xz" suffix.

If you want to use this GnuPG versions with Gpg4win simply install it on
on top of Gpg4win 3.1.16.


Checking the Integrity
======================

In order to check that the version of GnuPG which you are going to
install is an original and unmodified one, you can do it in one of
the following ways:

 * If you already have a version of GnuPG installed, you can simply
   verify the supplied signature.  For example to verify the signature
   of the file gnupg-2.3.2.tar.bz2 you would use this command:

     gpg --verify gnupg-2.3.2.tar.bz2.sig gnupg-2.3.2.tar.bz2

   This checks whether the signature file matches the source file.
   You should see a message indicating that the signature is good and
   made by one or more of the release signing keys.  Make sure that
   this is a valid key, either by matching the shown fingerprint
   against a trustworthy list of valid release signing keys or by
   checking that the key has been signed by trustworthy other keys.
   See the end of this mail for information on the signing keys.

 * If you are not able to use an existing version of GnuPG, you have
   to verify the SHA-1 checksum.  On Unix systems the command to do
   this is either "sha1sum" or "shasum".  Assuming you downloaded the
   file gnupg-2.3.2.tar.bz2, you run the command like this:

     sha1sum gnupg-2.3.2.tar.bz2

   and check that the output matches the next line:

f0f44b29e3702d3be1b25c964dfc2aaefe70e2fc  gnupg-2.3.2.tar.bz2
7c8ff377310f9781b89efb06222e7e74a19ed477  gnupg-w32-2.3.2_20210824.tar.xz
f6c1591e06ee2801961e216f44bc67243e87576f  gnupg-w32-2.3.2_20210824.exe


Internationalization
====================

This version of GnuPG has support for 26 languages with Chinese
(traditional and simplified), Czech, French, German, Italian,
Japanese, Norwegian, Polish, Russian, and Ukrainian being almost
completely translated.


Documentation and Support
=========================

The file gnupg.info has the complete reference manual of the system.
Separate man pages are included as well but they miss some of the
details available only in the manual.  The manual is also available
online at

  https://gnupg.org/documentation/manuals/gnupg/

or can be downloaded as PDF at

  https://gnupg.org/documentation/manuals/gnupg.pdf

You may also want to search the GnuPG mailing list archives or ask on
the gnupg-users mailing list for advise on how to solve problems.  Most
of the new features are around for several years and thus enough public
experience is available.  https://wiki.gnupg.org has user contributed
information around GnuPG and relate software.

In case of build problems specific to this release please first check
https://dev.gnupg.org/T5405 for updated information.

Please consult the archive of the gnupg-users mailing list before
reporting a bug: https://gnupg.org/documentation/mailing-lists.html.
We suggest to send bug reports for a new release to this list in favor
of filing a bug at https://bugs.gnupg.org.  If you need commercial
support go to https://gnupg.com or https://gnupg.org/service.html.

If you are a developer and you need a certain feature for your project,
please do not hesitate to bring it to the gnupg-devel mailing list for
discussion.


Thanks
======

Since 2001 maintenance and development of GnuPG is done by g10 Code GmbH
and still mostly financed by donations.  Three full-time employed
developers as well as two contractors exclusively work on GnuPG and
closely related software like Libgcrypt, GPGME and Gpg4win.

We like to thank all the nice people who are helping the GnuPG project,
be it testing, coding, translating, suggesting, auditing, administering
the servers, spreading the word, or answering questions on the mailing
lists.

The financial support of the governmental CERT of Luxembourg
(GOVCERT.LU) allowed us to develop new and improved features for
smartcards (Yubikey, PIV and Scute) as well as various usability
features.  Thanks.

Many thanks also to all other financial supporters, both corporate and
individuals.  Without you it would not be possible to keep GnuPG in a
good and secure shape and to address all the small and larger requests
made by our users.


Happy hacking,

   Your GnuPG hackers


p.s.
This is an announcement only mailing list.  Please send replies only to
the gnupg-users at gnupg.org mailing list.

p.p.s
List of Release Signing Keys:
To guarantee that a downloaded GnuPG version has not been tampered by
malicious entities we provide signature files for all tarballs and
binary versions.  The keys are also signed by the long term keys of
their respective owners.  Current releases are signed by one or more
of these four keys:

  ed25519 2020-08-24 [expires: 2030-06-30]
  Key fingerprint = 6DAA 6E64 A76D 2840 571B  4902 5288 97B8 2640 3ADA
  Werner Koch (dist signing 2020)

  rsa3072 2017-03-17 [expires: 2027-03-15]
  Key fingerprint = 5B80 C575 4298 F0CB 55D8  ED6A BCEF 7E29 4B09 2E28
  Andre Heinecke (Release Signing Key)

  rsa2048 2011-01-12 [expires: 2021-12-31]
  Key fingerprint = D869 2123 C406 5DEA 5E0F  3AB5 249B 39D2 4F25 E3B6
  Werner Koch (dist sig)

The keys are available at https://gnupg.org/signature_key.html and
in any recently released GnuPG tarball in the file g10/distsigkey.gpg .
Note that this mail has been signed by a different key.

--
Please read

  Nils Melzer: Der Fall Julian Assange

It is really important to know the background of the Assange case to
understand the massive perils to free journalism.  The book is right
now only available in German: https://dev.gnupg.org/u/melzerassang
-------------- next part --------------
A non-text attachment was scrubbed...
Name: signature.asc
Type: application/pgp-signature
Size: 227 bytes
Desc: not available
URL: <https://lists.gnupg.org/pipermail/gnupg-devel/attachments/20210824/b7d21edb/attachment-0001.sig>
-------------- next part --------------
_______________________________________________
Gnupg-announce mailing list
Gnupg-announce at gnupg.org
http://lists.gnupg.org/mailman/listinfo/gnupg-announce

From wk at gnupg.org  Fri Aug 27 15:08:19 2021
From: wk at gnupg.org (Werner Koch)
Date: Fri, 27 Aug 2021 15:08:19 +0200
Subject: [Announce] GnuPG 2.2.30 (LTS) released
Message-ID: <87mtp3qblo.fsf@wheatstone.g10code.de>

Hello!

We are pleased to announce the availability of a new GnuPG LTS release:
version 2.2.30.  This release fixes a few minor bugs and improves the
usability of symmetric-only encryption.  The LTS (long term support)
series of GnuPG is guaranteed to be maintained at least until the end
of 2024.  See https://gnupg.org/download/index.html#end-of-life


What is GnuPG
=============

The GNU Privacy Guard (GnuPG, GPG) is a complete and free implementation
of the OpenPGP and S/MIME standards.

GnuPG allows to encrypt and sign data and communication, features a
versatile key management system as well as access modules for public key
directories.  GnuPG itself is a command line tool with features for easy
integration with other applications.  The separate library GPGME provides
a uniform API to use the GnuPG engine by software written in common
programming languages.  A wealth of frontend applications and libraries
making use of GnuPG are available.  As an universal crypto engine GnuPG
provides support for S/MIME and Secure Shell in addition to OpenPGP.

GnuPG is Free Software (meaning that it respects your freedom).  It can
be freely used, modified and distributed under the terms of the GNU
General Public License.


Noteworthy changes in version 2.2.30 (2021-08-26)
=================================================

  * gpg: Extended gpg-check-pattern to support accept rules,
    conjunctions, and case-sensitive matching.  [5ca15e58b2]

  * agent: New option --pinentry-formatted-passphrase.  [#5553]

  * agent: New option --check-sym-passphrase-pattern.  [#5517]

  * agent: Use the sysconfdir for the pattern files.  [5ed8e598fa]

  * agent: Add "checkpin" inquiry for use by pinentry.  [#5532]

  * wkd: Fix client issue with leading or trailing spaces in
    user-ids.  [576e429d41]

  * Pass XDG_SESSION_TYPE and QT_QPA_PLATFORM envvars to Pinentry.
    [#3659]

  * Under Windows use LOCAL_APPDATA for the socket directory.  [#5537]

  Release-info: https://dev.gnupg.org/T5519


Getting the Software
====================

Please follow the instructions found at <https://gnupg.org/download/> or
read on:

GnuPG 2.2.30 may be downloaded from one of the GnuPG mirror sites or
direct from its primary FTP server.  The list of mirrors can be found at
<https://gnupg.org/download/mirrors.html>.  Note that GnuPG is not
available at ftp.gnu.org.

The GnuPG source code compressed using BZIP2 and its OpenPGP signature
are available here:

 https://gnupg.org/ftp/gcrypt/gnupg/gnupg-2.2.30.tar.bz2 (7046k)
 https://gnupg.org/ftp/gcrypt/gnupg/gnupg-2.2.30.tar.bz2.sig

An installer for Windows without any graphical frontend except for a
very minimal Pinentry tool is available here:

 https://gnupg.org/ftp/gcrypt/binary/gnupg-w32-2.2.30_20210826.exe (4393k)
 https://gnupg.org/ftp/gcrypt/binary/gnupg-w32-2.2.30_20210826.exe.sig

The source used to build the Windows installer can be found in the same
directory with a ".tar.xz" suffix.

A new version of Gpg4win will probably not be published.  Users affected
by one of the fixed bugs may instead install this version on top of
Gpg4win 3.1.16.


Checking the Integrity
======================

In order to check that the version of GnuPG which you are going to
install is an original and unmodified one, you can do it in one of
the following ways:

 * If you already have a version of GnuPG installed, you can simply
   verify the supplied signature.  For example to verify the signature
   of the file gnupg-2.2.30.tar.bz2 you would use this command:

     gpg --verify gnupg-2.2.30.tar.bz2.sig gnupg-2.2.30.tar.bz2

   This checks whether the signature file matches the source file.
   You should see a message indicating that the signature is good and
   made by one or more of the release signing keys.  Make sure that
   this is a valid key, either by matching the shown fingerprint
   against a trustworthy list of valid release signing keys or by
   checking that the key has been signed by trustworthy other keys.
   See the end of this mail for information on the signing keys.

 * If you are not able to use an existing version of GnuPG, you have
   to verify the SHA-1 checksum.  On Unix systems the command to do
   this is either "sha1sum" or "shasum".  Assuming you downloaded the
   file gnupg-2.2.30.tar.bz2, you run the command like this:

     sha1sum gnupg-2.2.30.tar.bz2

   and check that the output matches the next line:

bf9908b1a36b2bd1a4b9f0890198e8fb2d77a91c  gnupg-2.2.30.tar.bz2
2de8c54af9387c1068cba06ef23ac197dac8ced0  gnupg-w32-2.2.30_20210826.tar.xz
f576c2b194c609bfd0386569ef3b51dd2eb38a12  gnupg-w32-2.2.30_20210826.exe


Internationalization
====================

This version of GnuPG has support for 26 languages with Chinese
(traditional and simplified), Czech, French, German, Italian,
Japanese, Norwegian, Polish, Russian, and Ukrainian being almost
completely translated.


Documentation and Support
=========================

The file gnupg.info has the complete reference manual of the system.
Separate man pages are included as well but they miss some of the
details available only in thee manual.  The manual is also available
online at

  https://gnupg.org/documentation/manuals/gnupg/

or can be downloaded as PDF at

  https://gnupg.org/documentation/manuals/gnupg.pdf .

You may also want to search the GnuPG mailing list archives or ask on
the gnupg-users mailing list for advise on how to solve problems.  Most
of the new features are around for several years and thus enough public
experience is available.  https://wiki.gnupg.org has user contributed
information around GnuPG and relate software.

In case of build problems specific to this release please first check
https://dev.gnupg.org/T5519 for updated information.

Please consult the archive of the gnupg-users mailing list before
reporting a bug: https://gnupg.org/documentation/mailing-lists.html.
We suggest to send bug reports for a new release to this list in favor
of filing a bug at https://bugs.gnupg.org.  If you need commercial
support go to https://gnupg.com or https://gnupg.org/service.html.

If you are a developer and you need a certain feature for your project,
please do not hesitate to bring it to the gnupg-devel mailing list for
discussion.


Thanks
======

Since 2001 maintenance and development of GnuPG is done by g10 Code GmbH
and still mostly financed by donations.  Three full-time employed
developers as well as two contractors exclusively work on GnuPG and
closely related software like Libgcrypt, GPGME and Gpg4win.

We like to thank all the nice people who are helping the GnuPG project,
be it testing, coding, translating, suggesting, auditing, administering
the servers, spreading the word, or answering questions on the mailing
lists.

Many thanks to our numerous financial supporters, both corporate and
individuals.  Without you it would not be possible to keep GnuPG in a
good and secure shape and to address all the small and larger requests
made by our users.  Thanks.


Happy hacking,

   Your GnuPG hackers


p.s.
This is an announcement only mailing list.  Please send replies only to
the gnupg-users'at'gnupg.org mailing list.

List of Release Signing Keys:
To guarantee that a downloaded GnuPG version has not been tampered by
malicious entities we provide signature files for all tarballs and
binary versions.  The keys are also signed by the long term keys of
their respective owners.  Current releases are signed by one or more
of these four keys:

  ed25519 2020-08-24 [expires: 2030-06-30]
  Key fingerprint = 6DAA 6E64 A76D 2840 571B  4902 5288 97B8 2640 3ADA
  Werner Koch (dist signing 2020)

  rsa3072 2017-03-17 [expires: 2027-03-15]
  Key fingerprint = 5B80 C575 4298 F0CB 55D8  ED6A BCEF 7E29 4B09 2E28
  Andre Heinecke (Release Signing Key)

  rsa2048 2011-01-12 [expires: 2021-12-31]
  Key fingerprint = D869 2123 C406 5DEA 5E0F  3AB5 249B 39D2 4F25 E3B6
  Werner Koch (dist sig)

The keys are available at https://gnupg.org/signature_key.html and
in any recently released GnuPG tarball in the file g10/distsigkey.gpg .
Note that this mail has been signed by a different key.

-- 
Please read

  Nils Melzer: Der Fall Julian Assange

It is really important to know the background of the Assange case to
understand the massive perils to free journalism.  The book is right
now only available in German: https://dev.gnupg.org/u/melzerassang
-------------- next part --------------
A non-text attachment was scrubbed...
Name: signature.asc
Type: application/pgp-signature
Size: 227 bytes
Desc: not available
URL: <https://lists.gnupg.org/pipermail/gnupg-devel/attachments/20210827/92b21111/attachment.sig>
-------------- next part --------------
_______________________________________________
Gnupg-announce mailing list
Gnupg-announce at gnupg.org
http://lists.gnupg.org/mailman/listinfo/gnupg-announce

