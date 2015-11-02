[![Build Status](https://travis-ci.org/kenglxn/QRGen.png?branch=master)](https://travis-ci.org/kenglxn/QRGen)
[![Maven Central](https://maven-badges.herokuapp.com/maven-central/net.glxn.qrgen/core/badge.svg)](https://maven-badges.herokuapp.com/maven-central/net.glxn.qrgen/core)

[![Donate](https://rawgithub.com/twolfson/gittip-badge/0.2.0/dist/gittip.png)](https://www.gittip.com/kenglxn/)

<script data-gittip-username="kenglxn" data-gittip-widget="button" src="//gttp.co/v1.js">
</script>

### QRGen: a simple QRCode generation api for java built on top ZXING

#### Dependencies:

ZXING: http://code.google.com/p/zxing/

#### Get it:

QRGen consists of three modules: ```core```, ```javase``` and ```android```. Everything is available from [Maven Central Repository](http://search.maven.org/#browse%7C-852965118).

_**NOTE: SNAPSHOT versions are not available through Maven Central. If you want the snapshot version either build locally or add the Sonatype Snapshot repo to your pom or gradle file:**_

Maven:

    <repositories>
      <repository>
        <id>sonatype-snapshots</id>
        <name>sonatype-snapshots</name>
        <url>https://oss.sonatype.org/content/repositories/snapshots/</url>
      </repository>
    </repositories>

Gradle:

    repositories {
        maven {
            url "https://oss.sonatype.org/content/repositories/snapshots/"
        }
    }
##### Java Application

[![Maven Central](https://maven-badges.herokuapp.com/maven-central/net.glxn.qrgen/javase/badge.svg)](https://maven-badges.herokuapp.com/maven-central/net.glxn.qrgen/javase)

When developing a Java application you need to add ```javase``` module to your list of dependencies. The required ```core``` module will be added automatically by your build system:


Gradle:

    dependencies {
		compile ("net.glxn.qrgen:javase:2.0")
    }

Maven:

    <dependencies>
        <dependency>
            <groupId>net.glxn.qrgen</groupId>
            <artifactId>javase</artifactId>
            <version>2.0</version>
        </dependency>
    </dependencies>

##### Android

[![Maven Central](https://maven-badges.herokuapp.com/maven-central/net.glxn.qrgen/android/badge.svg)](https://maven-badges.herokuapp.com/maven-central/net.glxn.qrgen/android)

When you want to use QRGen inside your android application you need to add the ```android``` module to your list of dependencies. The required ```core``` module will be added automatically by your build system:

Gradle:

    dependencies {
		compile ("net.glxn.qrgen:android:2.0")
    }

Maven:

    <dependencies>
        <dependency>
            <groupId>net.glxn.qrgen</groupId>
            <artifactId>android</artifactId>
            <version>2.0</version>
        </dependency>
    </dependencies>

Or you can clone and build yourself:

    git clone git://github.com/kenglxn/QRGen.git
    cd QRGen/
    mvn clean install

#### Usage:

```java
// get QR file from text using defaults
File file = QRCode.from("Hello World").file();

// get QR stream from text using defaults
ByteArrayOutputStream stream = QRCode.from("Hello World").stream();

// override the image type to be JPG
QRCode.from("Hello World").to(ImageType.JPG).file();
QRCode.from("Hello World").to(ImageType.JPG).stream();

// override image size to be 250x250
QRCode.from("Hello World").withSize(250, 250).file();
QRCode.from("Hello World").withSize(250, 250).stream();

// override size and image type
QRCode.from("Hello World").to(ImageType.GIF).withSize(250, 250).file();
QRCode.from("Hello World").to(ImageType.GIF).withSize(250, 250).stream();

// override default colors (black on white)
// notice that the color format is "0x(alpha: 1 byte)(RGB: 3 bytes)"
// so in the example below it's red for foreground and yellowish for background, both 100% alpha (FF).
QRCode.from("Hello World").withColor(0xFFFF0000, 0xFFFFFFAA).file();

// supply own outputstream
QRCode.from("Hello World").to(ImageType.PNG).writeTo(outputStream);

// supply own file name
QRCode.from("Hello World").file("QRCode");

// supply charset hint to ZXING
QRCode.from("Hello World").withCharset("UTF-8");

// supply error correction level hint to ZXING
QRCode.from("Hello World").withErrorCorrection(ErrorCorrectionLevel.L);

// supply any hint to ZXING
QRCode.from("Hello World").withHint(EncodeHintType.CHARACTER_SET, "UTF-8");

// encode contact data as vcard using defaults
VCard johnDoe = new VCard("John Doe")
                    .setEmail("john.doe@example.org")
                    .setAddress("John Doe Street 1, 5678 Doestown")
                    .setTitle("Mister")
                    .setCompany("John Doe Inc.")
                    .setPhoneNumber("1234")
                    .setWebsite("www.example.org");
QRCode.from(johnDoe).file();

// if using special characters don't forget to supply the encoding
VCard johnSpecial = new VCard("Jöhn Dɵe")
                        .setAddress("ëåäöƞ Sträät 1, 1234 Döestüwn");
QRCode.from(johnSpecial).withCharset("UTF-8").file();

```

#### Android only

On Android you have a special method `bitmap()` which returns a `android.graphics.Bitmap` without creating a `File` object before, so you can use the generated `android.graphics.Bitmap` immediately inside an `ImageView`:

```java
Bitmap myBitmap = QRCode.from("www.example.org").bitmap();
ImageView myImage = (ImageView) findViewById(R.id.imageView);
myImage.setImageBitmap(myBitmap);
```

#### License:

http://www.apache.org/licenses/LICENSE-2.0.html


### Creating a new release

Since releasing to sonatype and central is such a pain, we now use bintray instead.

    mvn -Prelease clean install -s ./bintray-settings.xml
    mvn release:prepare -s ./bintray-settings.xml
    mvn release:perform -s ./bintray-settings.xml
    # go to bintray and synt to central
