---
layout:     post
title:      Scala Code Style Guideline
subtitle:   
date:       2017-01-05
author:     Eric Cheng
header-img: img/post-bg-coffee.jpeg
catalog: true
tags:
    - scala
---

## **Overview**

------



This document is help for you to write better Scala project. Before you start to read the rest of the document, please ensure you have good knowledge about Scala & SBT. 

We will introduce how to use SBT and Scala style checker plugin to help you build better Scala project.

## **Setup SBT scala style plugin**

------



Add the follow plugin in your project directory, like **stylecheck.sbt** :



```scala
addSbtPlugin("org.scalastyle" %% "scalastyle-sbt-plugin" % "1.0.0")
```



Added the config that checking style before compile in the **build.sbt** file::

```scala
lazy val compileScalastyle = taskKey[Unit]("compileScalastyle")
 
// scalastyle >= 0.9.0
compileScalastyle := scalastyle.in(Compile).toTask("").value
 
(compile in Compile) := ((compile in Compile) dependsOn compileScalastyle).value
```



Style check command::



```
sbt scalastyle
```


This produces a list of errors on the console, as well as an XML result file `target/scalastyle-result.xml` (CheckStyle compatible format).

## Added scala style config

------



Add the **scalastyle-config.xml** in your root directory:



```xml
<!--
  ~ Licensed to the Apache Software Foundation (ASF) under one or more
  ~ contributor license agreements.  See the NOTICE file distributed with
  ~ this work for additional information regarding copyright ownership.
  ~ The ASF licenses this file to You under the Apache License, Version 2.0
  ~ (the "License"); you may not use this file except in compliance with
  ~ the License.  You may obtain a copy of the License at
  ~
  ~    http://www.apache.org/licenses/LICENSE-2.0
  ~
  ~ Unless required by applicable law or agreed to in writing, software
  ~ distributed under the License is distributed on an "AS IS" BASIS,
  ~ WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
  ~ See the License for the specific language governing permissions and
  ~ limitations under the License.
  -->
<!--
 
If you wish to turn off checking for a section of code, you can put a comment in the source
before and after the section, with the following syntax:
 
  // scalastyle:off
  ...  // stuff that breaks the styles
  // scalastyle:on
 
You can also disable only one rule, by specifying its rule id, as specified in:
  http://www.scalastyle.org/rules-0.7.0.html
 
  // scalastyle:off no.finalize
  override def finalize(): Unit = ...
  // scalastyle:on no.finalize
 
This file is divided into 3 sections:
 (1) rules that we enforce.
 (2) rules that we would like to enforce, but haven't cleaned up the codebase to turn on yet
     (or we need to make the scalastyle rule more configurable).
 (3) rules that we don't want to enforce.
-->
 
<scalastyle>
    <name>Scalastyle standard configuration</name>
 
    <!-- ================================================================================ -->
    <!--                               rules we enforce                                   -->
    <!-- ================================================================================ -->
 
    <check level="error" class="org.scalastyle.file.FileTabChecker" enabled="true"></check>
 
    <check level="error" class="org.scalastyle.scalariform.SpacesAfterPlusChecker" enabled="true"></check>
 
    <check level="error" class="org.scalastyle.scalariform.SpacesBeforePlusChecker" enabled="true"></check>
 
    <check level="error" class="org.scalastyle.file.WhitespaceEndOfLineChecker" enabled="true"></check>
 
    <check level="error" class="org.scalastyle.file.FileLineLengthChecker" enabled="true">
        <parameters>
            <parameter name="maxLineLength"><![CDATA[120]]></parameter>
            <parameter name="tabSize"><![CDATA[2]]></parameter>
            <parameter name="ignoreImports">true</parameter>
        </parameters>
    </check>
 
    <check enabled="true" class="org.scalastyle.file.HeaderMatchesChecker" level="error">
        <parameters>
            <parameter name="regex">true</parameter>
            <parameter name="header"><![CDATA[/\* Order Mart Team \-\- .*]]></parameter>
        </parameters>
    </check>
 
    <check level="error" class="org.scalastyle.scalariform.ClassNamesChecker" enabled="true">
        <parameters>
            <parameter name="regex"><![CDATA[[A-Z][A-Za-z]*]]></parameter>
        </parameters>
    </check>
 
    <check level="error" class="org.scalastyle.scalariform.ObjectNamesChecker" enabled="true">
        <parameters>
            <parameter name="regex"><![CDATA[(config|[A-Z][A-Za-z]*)]]></parameter>
        </parameters>
    </check>
 
    <check level="error" class="org.scalastyle.scalariform.PackageObjectNamesChecker" enabled="true">
        <parameters>
            <parameter name="regex"><![CDATA[^[a-z][A-Za-z]*$]]></parameter>
        </parameters>
    </check>
 
    <check customId="argcount" level="error" class="org.scalastyle.scalariform.ParameterNumberChecker" enabled="true">
        <parameters>
            <parameter name="maxParameters"><![CDATA[10]]></parameter>
        </parameters>
    </check>
 
    <check level="error" class="org.scalastyle.scalariform.NoFinalizeChecker" enabled="true"></check>
 
    <check level="error" class="org.scalastyle.scalariform.CovariantEqualsChecker" enabled="true"></check>
 
    <check level="error" class="org.scalastyle.scalariform.StructuralTypeChecker" enabled="true"></check>
 
    <check level="error" class="org.scalastyle.scalariform.UppercaseLChecker" enabled="true"></check>
 
    <check level="error" class="org.scalastyle.scalariform.IfBraceChecker" enabled="true">
        <parameters>
            <parameter name="singleLineAllowed"><![CDATA[true]]></parameter>
            <parameter name="doubleLineAllowed"><![CDATA[true]]></parameter>
        </parameters>
    </check>
 
    <check level="error" class="org.scalastyle.scalariform.PublicMethodsHaveTypeChecker" enabled="true"></check>
 
    <check level="error" class="org.scalastyle.file.NewLineAtEofChecker" enabled="true"></check>
 
    <check customId="nonascii" level="error" class="org.scalastyle.scalariform.NonASCIICharacterChecker"
           enabled="true"></check>
 
    <check level="error" class="org.scalastyle.scalariform.SpaceAfterCommentStartChecker" enabled="true"></check>
 
    <check level="error" class="org.scalastyle.scalariform.EnsureSingleSpaceBeforeTokenChecker" enabled="true">
        <parameters>
            <parameter name="tokens">ARROW, EQUALS, ELSE, TRY, CATCH, FINALLY, LARROW, RARROW</parameter>
        </parameters>
    </check>
 
    <check level="error" class="org.scalastyle.scalariform.EnsureSingleSpaceAfterTokenChecker" enabled="true">
        <parameters>
            <parameter name="tokens">ARROW, EQUALS, COMMA, COLON, IF, ELSE, DO, WHILE, FOR, MATCH, TRY, CATCH, FINALLY,
                LARROW, RARROW
            </parameter>
        </parameters>
    </check>
 
    <!-- ??? usually shouldn't be checked into the code base. -->
    <check level="error" class="org.scalastyle.scalariform.NotImplementedErrorUsage" enabled="true"></check>
 
 
    <!-- As of SPARK-7977 all printlns need to be wrapped in '// scalastyle:off/on println' -->
    <check customId="println" level="error" class="org.scalastyle.scalariform.TokenChecker" enabled="true">
        <parameters>
            <parameter name="regex">^println$</parameter>
        </parameters>
        <customMessage><![CDATA[Are you sure you want to println? If yes, wrap the code block with
      // scalastyle:off println
      println(...)
      // scalastyle:on println]]></customMessage>
    </check>
 
    <check customId="hadoopconfiguration" level="error" class="org.scalastyle.file.RegexChecker" enabled="true">
        <parameters>
            <parameter name="regex">spark(.sqlContext)?.sparkContext.hadoopConfiguration</parameter>
        </parameters>
        <customMessage><![CDATA[
      Are you sure that you want to use sparkContext.hadoopConfiguration? In most cases, you should use
      spark.sessionState.newHadoopConf() instead, so that the hadoop configurations specified in Spark session
      configuration will come into effect.
      If you must use sparkContext.hadoopConfiguration, wrap the code block with
      // scalastyle:off hadoopconfiguration
      spark.sparkContext.hadoopConfiguration...
      // scalastyle:on hadoopconfiguration
    ]]></customMessage>
    </check>
 
    <check customId="visiblefortesting" level="error" class="org.scalastyle.file.RegexChecker" enabled="true">
        <parameters>
            <parameter name="regex">@VisibleForTesting</parameter>
        </parameters>
        <customMessage><![CDATA[
      @VisibleForTesting causes classpath issues. Please note this in the java doc instead (SPARK-11615).
    ]]></customMessage>
    </check>
 
    <check customId="runtimeaddshutdownhook" level="error" class="org.scalastyle.file.RegexChecker" enabled="true">
        <parameters>
            <parameter name="regex">Runtime\.getRuntime\.addShutdownHook</parameter>
        </parameters>
        <customMessage><![CDATA[
      Are you sure that you want to use Runtime.getRuntime.addShutdownHook? In most cases, you should use
      ShutdownHookManager.addShutdownHook instead.
      If you must use Runtime.getRuntime.addShutdownHook, wrap the code block with
      // scalastyle:off runtimeaddshutdownhook
      Runtime.getRuntime.addShutdownHook(...)
      // scalastyle:on runtimeaddshutdownhook
    ]]></customMessage>
    </check>
 
    <check customId="mutablesynchronizedbuffer" level="error" class="org.scalastyle.file.RegexChecker" enabled="true">
        <parameters>
            <parameter name="regex">mutable\.SynchronizedBuffer</parameter>
        </parameters>
        <customMessage><![CDATA[
      Are you sure that you want to use mutable.SynchronizedBuffer? In most cases, you should use
      java.util.concurrent.ConcurrentLinkedQueue instead.
      If you must use mutable.SynchronizedBuffer, wrap the code block with
      // scalastyle:off mutablesynchronizedbuffer
      mutable.SynchronizedBuffer[...]
      // scalastyle:on mutablesynchronizedbuffer
    ]]></customMessage>
    </check>
 
    <check customId="classforname" level="error" class="org.scalastyle.file.RegexChecker" enabled="true">
        <parameters>
            <parameter name="regex">Class\.forName</parameter>
        </parameters>
        <customMessage><![CDATA[
      Are you sure that you want to use Class.forName? In most cases, you should use Utils.classForName instead.
      If you must use Class.forName, wrap the code block with
      // scalastyle:off classforname
      Class.forName(...)
      // scalastyle:on classforname
    ]]></customMessage>
    </check>
 
    <check customId="awaitresult" level="error" class="org.scalastyle.file.RegexChecker" enabled="true">
        <parameters>
            <parameter name="regex">Await\.result</parameter>
        </parameters>
        <customMessage><![CDATA[
      Are you sure that you want to use Await.result? In most cases, you should use ThreadUtils.awaitResult instead.
      If you must use Await.result, wrap the code block with
      // scalastyle:off awaitresult
      Await.result(...)
      // scalastyle:on awaitresult
    ]]></customMessage>
    </check>
 
    <check customId="awaitready" level="error" class="org.scalastyle.file.RegexChecker" enabled="true">
        <parameters>
            <parameter name="regex">Await\.ready</parameter>
        </parameters>
        <customMessage><![CDATA[
      Are you sure that you want to use Await.ready? In most cases, you should use ThreadUtils.awaitReady instead.
      If you must use Await.ready, wrap the code block with
      // scalastyle:off awaitready
      Await.ready(...)
      // scalastyle:on awaitready
    ]]></customMessage>
    </check>
 
    <check customId="caselocale" level="error" class="org.scalastyle.file.RegexChecker" enabled="true">
        <parameters>
            <parameter name="regex">(\.toUpperCase|\.toLowerCase)(?!(\(|\(Locale.ROOT\)))</parameter>
        </parameters>
        <customMessage><![CDATA[
      Are you sure that you want to use toUpperCase or toLowerCase without the root locale? In most cases, you
      should use toUpperCase(Locale.ROOT) or toLowerCase(Locale.ROOT) instead.
      If you must use toUpperCase or toLowerCase without the root locale, wrap the code block with
      // scalastyle:off caselocale
      .toUpperCase
      .toLowerCase
      // scalastyle:on caselocale
    ]]></customMessage>
    </check>
 
    <check customId="throwerror" level="error" class="org.scalastyle.file.RegexChecker" enabled="true">
        <parameters>
            <parameter name="regex">throw new \w+Error\(</parameter>
        </parameters>
        <customMessage><![CDATA[
      Are you sure that you want to throw Error? In most cases, you should use appropriate Exception instead.
      If you must throw Error, wrap the code block with
      // scalastyle:off throwerror
      throw new XXXError(...)
      // scalastyle:on throwerror
    ]]></customMessage>
    </check>
 
    <!-- As of SPARK-9613 JavaConversions should be replaced with JavaConverters -->
    <check customId="javaconversions" level="error" class="org.scalastyle.scalariform.TokenChecker" enabled="true">
        <parameters>
            <parameter name="regex">JavaConversions</parameter>
        </parameters>
        <customMessage>Instead of importing implicits in scala.collection.JavaConversions._, import
            scala.collection.JavaConverters._ and use .asScala / .asJava methods
        </customMessage>
    </check>
 
    <check customId="commonslang2" level="error" class="org.scalastyle.file.RegexChecker" enabled="true">
        <parameters>
            <parameter name="regex">org\.apache\.commons\.lang\.</parameter>
        </parameters>
        <customMessage>Use Commons Lang 3 classes (package org.apache.commons.lang3.*) instead
            of Commons Lang 2 (package org.apache.commons.lang.*)
        </customMessage>
    </check>
 
    <check customId="extractopt" level="error" class="org.scalastyle.scalariform.TokenChecker" enabled="true">
        <parameters>
            <parameter name="regex">extractOpt</parameter>
        </parameters>
        <customMessage>Use jsonOption(x).map(.extract[T]) instead of .extractOpt[T], as the latter
            is slower.
        </customMessage>
    </check>
 
    <check level="error" class="org.scalastyle.scalariform.ImportOrderChecker" enabled="true">
        <parameters>
            <parameter name="groups">java,scala,flink,spark,3rdParty,ordermart</parameter>
            <parameter name="group.java">javax?\..*</parameter>
            <parameter name="group.scala">scala\..*</parameter>
            <parameter name="group.flink">org\.apache\.flink\..*</parameter>
            <parameter name="group.spark">org\.apache\.spark\..*</parameter>
        </parameters>
    </check>
 
    <check level="error" class="org.scalastyle.scalariform.DisallowSpaceBeforeTokenChecker" enabled="true">
        <parameters>
            <parameter name="tokens">COMMA</parameter>
        </parameters>
    </check>
 
    <!-- SPARK-3854: Single Space between ')' and '{' -->
    <check customId="SingleSpaceBetweenRParenAndLCurlyBrace" level="error" class="org.scalastyle.file.RegexChecker"
           enabled="true">
        <parameters>
            <parameter name="regex">\)\{</parameter>
        </parameters>
        <customMessage><![CDATA[
      Single Space between ')' and `{`.
    ]]></customMessage>
    </check>
 
    <check customId="NoScalaDoc" level="error" class="org.scalastyle.file.RegexChecker" enabled="true">
        <parameters>
            <parameter name="regex">(?m)^(\s*)/[*][*].*$(\r|)\n^\1 [*]</parameter>
        </parameters>
        <customMessage>Use Javadoc style indentation for multiline comments</customMessage>
    </check>
 
    <check customId="OmitBracesInCase" level="error" class="org.scalastyle.file.RegexChecker" enabled="true">
        <parameters>
            <parameter name="regex">case[^\n>]*=>\s*\{</parameter>
        </parameters>
        <customMessage>Omit braces in case clauses.</customMessage>
    </check>
 
    <!-- SPARK-16877: Avoid Java annotations -->
    <check level="error" class="org.scalastyle.scalariform.OverrideJavaChecker" enabled="true"></check>
 
    <check level="error" class="org.scalastyle.scalariform.DeprecatedJavaChecker" enabled="true"></check>
 
    <!-- ================================================================================ -->
    <!--       rules we'd like to enforce, but haven't cleaned up the codebase yet        -->
    <!-- ================================================================================ -->
 
    <!-- We cannot turn the following two on, because it'd fail a lot of string interpolation use cases. -->
    <!-- Ideally the following two rules should be configurable to rule out string interpolation. -->
    <check level="error" class="org.scalastyle.scalariform.NoWhitespaceBeforeLeftBracketChecker"
           enabled="false"></check>
    <check level="error" class="org.scalastyle.scalariform.NoWhitespaceAfterLeftBracketChecker" enabled="false"></check>
 
    <!-- This breaks symbolic method names so we don't turn it on. -->
    <!-- Maybe we should update it to allow basic symbolic names, and then we are good to go. -->
    <check level="error" class="org.scalastyle.scalariform.MethodNamesChecker" enabled="false">
        <parameters>
            <parameter name="regex"><![CDATA[^[a-z][A-Za-z0-9]*$]]></parameter>
        </parameters>
    </check>
 
    <!-- Should turn this on, but we have a few places that need to be fixed first -->
    <check level="error" class="org.scalastyle.scalariform.EqualsHashCodeChecker" enabled="false"></check>
 
    <!-- ================================================================================ -->
    <!--                               rules we don't want                                -->
    <!-- ================================================================================ -->
 
    <check level="error" class="org.scalastyle.scalariform.IllegalImportsChecker" enabled="false">
        <parameters>
            <parameter name="illegalImports"><![CDATA[sun._,java.awt._]]></parameter>
        </parameters>
    </check>
 
    <!-- We want the opposite of this: NewLineAtEofChecker -->
    <check level="error" class="org.scalastyle.file.NoNewLineAtEofChecker" enabled="false"></check>
 
    <!-- This one complains about all kinds of random things. Disable. -->
    <check level="error" class="org.scalastyle.scalariform.SimplifyBooleanExpressionChecker" enabled="false"></check>
 
    <!-- We use return quite a bit for control flows and guards -->
    <check level="error" class="org.scalastyle.scalariform.ReturnChecker" enabled="false"></check>
 
    <!-- We use null a lot in low level code and to interface with 3rd party code -->
    <check level="error" class="org.scalastyle.scalariform.NullChecker" enabled="false"></check>
 
    <!-- Doesn't seem super big deal here ... -->
    <check level="error" class="org.scalastyle.scalariform.NoCloneChecker" enabled="false"></check>
 
    <!-- Doesn't seem super big deal here ... -->
    <check level="error" class="org.scalastyle.file.FileLengthChecker" enabled="false">
        <parameters>
            <parameter name="maxFileLength">800></parameter>
        </parameters>
    </check>
 
    <!-- Doesn't seem super big deal here ... -->
    <check level="error" class="org.scalastyle.scalariform.NumberOfTypesChecker" enabled="false">
        <parameters>
            <parameter name="maxTypes">30</parameter>
        </parameters>
    </check>
 
    <!-- Doesn't seem super big deal here ... -->
    <check level="error" class="org.scalastyle.scalariform.CyclomaticComplexityChecker" enabled="false">
        <parameters>
            <parameter name="maximum">10</parameter>
        </parameters>
    </check>
 
    <!-- Doesn't seem super big deal here ... -->
    <check level="error" class="org.scalastyle.scalariform.MethodLengthChecker" enabled="false">
        <parameters>
            <parameter name="maxLength">50</parameter>
        </parameters>
    </check>
 
    <!-- Not exactly feasible to enforce this right now. -->
    <!-- It is also infrequent that somebody introduces a new class with a lot of methods. -->
    <check level="error" class="org.scalastyle.scalariform.NumberOfMethodsInTypeChecker" enabled="false">
        <parameters>
            <parameter name="maxMethods"><![CDATA[30]]></parameter>
        </parameters>
    </check>
 
    <!-- Doesn't seem super big deal here, and we have a lot of magic numbers ... -->
    <check level="error" class="org.scalastyle.scalariform.MagicNumberChecker" enabled="false">
        <parameters>
            <parameter name="ignore">-1,0,1,2,3</parameter>
        </parameters>
    </check>
 
</scalastyle>
```



## Setup IDEA

------

It is important to use a wonderful IDEA to write Scala project, I suggest you guys use IDEA or Eclipse to write Scala projects.

Install **Save Actions** plugin for IDEA, Search "Save Actions" in the IDEA Marketplace and install .



## Links

------

The links will help your know more about style config ::

[Scala Style Checker ](http://www.scalastyle.org/)

[SBT Style Check Plugin Help Documentation](http://www.scalastyle.org/sbt.html)

[Style Rules](http://www.scalastyle.org/rules-1.0.0.html)