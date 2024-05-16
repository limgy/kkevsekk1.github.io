# WorkWithJava

```js
/* -*- Mode: java; tab-width: 8; indent-tabs-mode: nil; c-basic-offset: 4 -*-
 *
 * This Source Code Form is subject to the terms of the Mozilla Public
 * License, v. 2.0. If a copy of the MPL was not distributed with this
 * file, You can obtain one at http://mozilla.org/MPL/2.0/. */

/**
 * liveConnect.js: a simple demonstration of JavaScript-to-Java connectivity
 */

// Create a new StringBuffer. Note that the class name must be fully qualified
// by its package. Packages other than "java" must start with "Packages", i.e.,
// "Packages.javax.servlet...".
var sb = new java.lang.StringBuffer();

// Now add some stuff to the buffer.
sb.append("hi, mom");
sb.append(3);	// this will add "3.0" to the buffer since all JS numbers
		// are doubles by default
sb.append(true);

// Now print it out. (The toString() method of sb is automatically called
// to convert the buffer to a string.)
// Should print "hi, mom3.0true".
print(sb);
openConsole();

```

关于调用 java [请查看这里](https://p-bakker.github.io/rhino/tutorials/scripting_java/) ，一个由 Rhino 维护者使用 GitHub Pages 部署的 Rhino 文档网站


[官方 issues 关于原 Rhino 文档地址404 的讨论](https://github.com/mozilla/rhino/issues/954#issuecomment-949763810)

