# Introduction #

Simple Java Mail is a lightweight wrapper framework for [Sun's JavaMail](http://java.sun.com/products/javamail/) (javax.mail API) for sending emails over smtp. Simple Java Mail is designed to be as easy to use as possible, while still being able to do everything you want when sending email.

This library has been specifically designed to be RFC compliant and produce e-mails of any complexity that are correctly interpreted by e-mail clients.

[Why Simple Java Mail being RFC compliant is important](RFC_Compliant.md)

**When using this library, you may refer to the [Javadoc for users](http://simple-java-mail.googlecode.com/svn/trunk/javadoc/users/index.html)** (But really, the example below should probably be enough).

# Required libraries #

Also see: [NOTICE.txt](http://code.google.com/p/simple-java-mail/source/browse/trunk/NOTICE.txt)

Because Simple Java Mail builds on the native JavaMail library, it has the same dependencies and adds log4j to it (although the sourcecode could be modified to use Java's built in logging framework).

Simple Java Mail uses a couple of jars that are all opensource. These are the Apache log4j library and Sun JavaMail API library (only the smtp sublibraries, version 1.3.2 and up including 1.4.4). It uses the javax.activation package as well, which is shipped with JavaEE or is available as seperate package.

  * [Log4j](http://logging.apache.org/log4j/1.2/index.html) (download [log4j.jar](http://simple-java-mail.googlecode.com/files/log4j-1.2.15.jar))
  * [JavaMail framework](http://www.oracle.com/technetwork/java/javamail/index.html) (download 1.4.4: [smtp.jar](http://simple-java-mail.googlecode.com/files/smtp-1.4.4.jar), [mailapi.jar](http://simple-java-mail.googlecode.com/files/mailapi-1.4.4.jar))
  * [Activation framework](http://java.sun.com/javase/technologies/desktop/javabeans/jaf/index.jsp) (included in jre1.6, or download [activation.jar](http://simple-java-mail.googlecode.com/files/activation.jar))

# Example usage #

For most use cases there are only three classes exposed to the public. These are [Email](http://simple-java-mail.googlecode.com/svn/trunk/javadoc/users/org/codemonkey/simplejavamail/Email.html), [Mailer](http://simple-java-mail.googlecode.com/svn/trunk/javadoc/users/org/codemonkey/simplejavamail/Mailer.html) and [MailException](http://simple-java-mail.googlecode.com/svn/trunk/javadoc/users/org/codemonkey/simplejavamail/MailException.html). Simply create and fill an email and then call `mailer.send(yourEmail)` and that's it. Optionally catch runtime MailExceptions.

Here's an example:

```
final Email email = new Email();

email.setFromAddress("lollypop", "lolly.pop@somemail.com");
email.setSubject("hey");
email.addRecipient("C. Cane", "candycane@candyshop.org", RecipientType.TO);
email.addRecipient("C. Bo", "chocobo@candyshop.org", RecipientType.BCC);
email.setText("We should meet up! ;)");
email.setTextHTML("<img src='cid:wink1'><b>We should meet up!</b><img src='cid:wink2'>");

// embed images and include downloadable attachments
email.addEmbeddedImage("wink1", imageByteArray, "image/png");
email.addEmbeddedImage("wink2", imageDatesource);
email.addAttachment("invitation", pdfByteArray, "application/pdf");
email.addAttachment("dresscode", odfDatasource);

new Mailer("smtp.host.com", 25, "username", "password").sendMail(email);
```

You can also pass in your own preconfigured mail _Session_ instance, so this framework is backwards compatible with whatever you use now.

```
new Mailer(yourSession).sendMail(email);
```

[Concerning the log4j warning](Log4jAppendersWarning.md)

### Using SSL / TLS ###

Using SSL (and TLS) is easy!

```
new Mailer("smtp.host.com", 25, "username", "password", TransportStrategy.SMTP_SSL).sendMail(email);
new Mailer("smtp.host.com", 25, "username", "password", TransportStrategy.SMTP_TLS).sendMail(email);
```

Note:
Although I've successfully used SMTP\_SLL with gmail's smtp server, I'm not convinced it works with self-signed ssl certificates as well (such as your own smtp server at home might use). The library may need a tweak to get that working as well. Let me know how this works out for you!

### Adding headers ###

```
Email email = new Email();
email.addHeader("X-Priority", 2);
```

### Additional validation Utilities ###

There are two more classes exposed for public use, concerning e-mail validation. [EmailValidationUtil](http://simple-java-mail.googlecode.com/svn/trunk/javadoc/users/org/codemonkey/simplejavamail/EmailValidationUtil.html) can be used to validate any e-mail aside from sending them. It is used internally, but if you want you can use this tool yourself as well. The validation methods optionally accept an instance of the second available class, [EmailAddressValidationCriteria](http://simple-java-mail.googlecode.com/svn/trunk/javadoc/users/org/codemonkey/simplejavamail/EmailAddressValidationCriteria.html), which influences how strict validation works.

# Alternatives #

There are some alternatives around, such as the [Spring mailing framework](http://static.springframework.org/spring/docs/2.0.x/reference/mail.html) or the [Apache Commons Mail](http://commons.apache.org/email/userguide.html). However, Spring works a bit awkward with anonymous subclasses and stuff (and you need Spring in your dependencies), while Apache Commons Mail isn't as simple and straightforward as it could be.

# Roadmap #

  * Support for proxy server

# Developing Simple Java Mail #

The sourcecode is relatively easy to understand and well commented. When you've checked out the code from the Subversion repository, you can run a build using `ant compile`, or `ant jar` if you like to create a jar while you're at it. To run the included test class use `ant` or `ant run`, which does the same. Make sure you provide some jvm arguments to indicate which smtp server to use for sending emails.

Example:
`ant -Dhost=smtp.someserver.com -Dport=25 -Dusername=joe -Dpassword=sixpack`

Also check out the developers version of the Javadoc:
  * [Javadoc for developers](http://simple-java-mail.googlecode.com/svn/trunk/javadoc/developers/index.html)

### Some factoids ###

Simple Java Mail's working title is Vesijama, which migrated from [vesijama.googlecode.com](http://code.google.com/p/vesijama/). Vesijama Stands for Very Simple Java Mail.

&lt;wiki:gadget url="http://www.ohloh.net/p/328803/widgets/project\_factoids.xml" border="0" width="400"/&gt;
&lt;wiki:gadget url="http://www.ohloh.net/p/328803/widgets/project\_languages.xml" border="0" width="400" height="200"/&gt;