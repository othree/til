# Sign

Git allows user to [sign][] a commit by PGP key. Github already [supports][github] this 
feature. Follow the document, you can easily sign your commits and tags.

[Linus Torvalds][so] don't suggest sign every commit. It makes youe signature worth less.
Since git already use sha1 to chain every thing(explaind a little in git [object][]). Signing tag is 
good enough to make sure the codes are verified.

But in some scenario, you might still need to sign every commit. You can add an option to `.gitconfig`:

    git config --global commit.gpgsign true

Remeber to use a PGP key with passphrase. Otherwise, it is really worth less signature.

A Normal commit looks like:

	tree 59cf2be5a155b6910edc3f32404a3715f5dadc39
	parent 971fdfb17f3281013c16b4e1323d0cc3688d57d3
	author othree <othree@gmail.com> 1459854311 +0000
	committer othree <othree@gmail.com> 1459854311 +0000
	
	Period task

And a signed commit looks like:

	tree 59d9bc4fe6f371cd29ac31fa36a22c6c5aa52bfc
	parent e27bc1a0026dbd4626b8daf5e781e30563b02d88
	author othree <othree@gmail.com> 1460287653 +0800
	committer othree <othree@gmail.com> 1460287653 +0800
	gpgsig -----BEGIN PGP SIGNATURE-----
	 Version: GnuPG v1
	
	 iQIcBAABAgAGBQJXCjitAAoJEPpbVOgX+o9SGXYP/2TYDnoTl9ReKOCm3DDnc9Tu
	 cjugXdUyTzayXMu47qUAgDGuOmAPgyudgjcG+OdN62lQHsGtLRraZACnz9yHzt79
	 fgZzEtJBsJOyDQb8JyWnMxl6SyMHs+jbyLeKqfHhxqwmG9JiUQFMeba8ZtEo+Tw2
	 yjXuHlU3VKx0+HVmQ4Zw2tk2kjifxTzQTHsen+u47VfCpQZUDYO3A8b7QMVsd+Sl
	 4BzOko18NpwkGqtpBoxtQLPHWVB0p0ZEw/V0N2YTFZ66sFtYkiuszwYRzaD6k/m6
	 8Vnr5MNvCvxUA7puNe3vDYKJ8Voou/Hwz4aSW6314BRNdGQRE91lHeNbn829gAGZ
	 CAFQJ+PZyPNtqOLSwV47crGde78LNA9f9AWlSN3ZuolyqiqzyZ4Amz3iMq86M8um
	 rnzyybimYLK9RLkzMDBy6j6332OV4QjCuGmnHJY72WDe28mEFFJzBvrqF5RICil/
	 0iVp875pIQEvqxeIiWaK7rTTaAs+9bQWi2XVUjn/mhe8eroPBdbQPJpOwbOXTEc2
	 s75LVxQjGricr7XfstU2zacnE695dKEyK04EHw2VDhaVrusXxMi5serYee0b3kCg
	 BllUsADagB6Znytxt9v33w+XFPppjaO+0aoFd4dnxvl8iGB2L2pNwmHnGFV3sorx
	 JRGYZTxnb2MkEIOG6Fyb
	 =9eYq
	 -----END PGP SIGNATURE-----
	
	Fix CSP for GA

[github]:https://github.com/blog/2144-gpg-signature-verification
[sign]:https://git-scm.com/book/zh-tw/v2/Git-Tools-Signing-Your-Work
[gpgsign]:https://git-scm.com/docs/git-config
[so]:http://stackoverflow.com/a/10166916
[object]:https://github.com/othree/til/blob/master/git/object.md