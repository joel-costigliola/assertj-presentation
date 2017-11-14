## AssertJ
##### <span style="font-family:Helvetica Neue; font-weight:bold">Rich and easy to use assertions</span>

http://joel-costigliola.github.io/assertj

---

## Agenda

- What is AssertJ ? |
- Quick start |
- Assertions quick tour |
- Soft assertions |
- Using conditions |
- AssertJ extensions |
- Advanced stuff |
- Hands on ! |

---

## What is AssertJ ?

AssertJ is an assertion java library which:
- provides rich assertions for common java types |
- improves code readability thanks to its fluent API | 
- easy to use with your favorite IDE |
- reports helpful error messages when assertions fail | 
- has javadoc with code examples |

---

## Quick start

Add assertj-core library to your test dependencies 

![Press Down Key](assets/down-arrow.png)

+++

#### PB2 Test project setup

PB2 Test project setup

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<ivy-module version="1.0">
  <info module="my.unit.tests" organisation="orchestral" status="integration"></info>
  <configurations>
    <conf name="test"/>
  </configurations>
  <publications>
  </publications>
  <dependencies>
    <dependency conf="test->nodist" name="assertj-core" 
                org="assertj" rev="3.8.0"/>
    <!-- other usual test dependencies -->
    <dependency conf="test->nodist" org="bundle" name="org.junit" rev="4.12.0"/>
    <dependency conf="test->nodist" org="bundle" name="org.mockito" rev="1.9.5"/>
  </dependencies>
</ivy-module>
```

@[5](ivy test config)
@[10-11](assertj dependency)

+++

#### Maven or Gradle setup

Maven

```xml
<dependency>
  <groupId>org.assertj</groupId>
  <artifactId>assertj-core</artifactId>
  <version>3.8.0</version>
  <scope>test</scope>
</dependency>
```

Gradle
```
testCompile 'org.assertj:assertj-core:3.8.0'
```

---

## Basic assertions

- Just start with the Assertions class |
- Allow chaining assertions  |
- IDE friendly | 
- Quick assertions demo |

Note:
Demo: 
* no more confusion about expected vs actual
* assertion description as()
* chaining assertion
* show representation ?

#### Example

Basic assertions example:

```java
assertThat("Gandalf")                      
    .as("Check a famous magician name")
    .isNotNull()                       
    .isInstanceOf(String.class)        
    .isNotEqualTo("Saroumane")         
    .isEqualTo("Gandalf");             
```

@[2](describe your assertion)
@[3-6](chain assertions)

---

## Collection assertions

- Works for Iterable, arrays and Stream |
- Allow chaining assertions  |
- IDE friendly | 
- Quick assertions demo |

Note:
* Stream are converted to List to allow multiple assertions since you only consume a Stream once.
* assertion description as()
* chaining assertion
* show representation ?


+++

#### Present Source Directly From Your Repo

<br>

Step through source code directly within your presentations.
*No more switching* back and forth between your slideshow and your IDE!

+++?code=src/elixir/monitor.ex&lang=elixir&title=Repo Source File: Elixir Snippets

@[11-14](Elixir module-attributes as constants)
@[22-28](Elixir with-statement for conciseness)
@[171-177](Elixir case-statement pattern matching)
@[179-185](Elixir pipe-mechanism for composing functions)=


```js
query {
  connection {
    status
    config {
      keyspace
      contacts 
    } 
  }
}
```

@[2](connection is a query)
@[3](status is a simple field)
@[4-7](config is an object field)



---

## Advanced assertions

- Use satisfies to group logical assertions |
- Use matches to check a logical condition |
- Using comparators |
- Using field by field comparison |

Note:
Demo: 
* no more confusion about expected vs actual
* assertion description as()
* chaining assertion
* show representation ?

---

## GraphQL Schema

- Schema = data structures + mutations  |
- Is the API the client needs from the server |
- Easily mocked by the client |
- Discoverable at runtime |  

---

## Query
<span style="font-size:0.6em; color:gray">Available bottom-left of screen.</span> |
<span style="font-size:0.6em; color:gray">Start switching themes right now!</span>

---

## Mutation
For the *best viewing experience*   
press **F** key to go fullscreen.

---

## Advantages over REST
For the *best viewing experience*   
press **F** key to go fullscreen.

- allow to retrieve multiple data structures with one "composite" query
- the client can select what to retrieve
- tooling

---

## Design considerations

- Use coarse grained data structures as the client can select a subpart of them
- Use rich data struxcure to describe mutation result (not a simple error code !)

---

## Pain points

- Java implementation constraints 
- Mutations 

---

## Demo
For the *best viewing experience*   
press **F** key to go fullscreen.

---


## Conclusion

Good way for client and server teams to agree on the API 
Easy to explore the schema and run queries with the existing tooling
Overall a good choice, better suited than REST API for our use case

---

## Markdown Slides
<span style="font-size:0.6em; color:gray">Press Down key for details.</span> |
<span style="font-size:0.6em; color:gray">See [GitPitch Wiki](https://github.com/gitpitch/gitpitch/wiki/Slide-Markdown) for details.</span>

![Press Down Key](assets/down-arrow.png)


+++

#### Use GitHub Flavored Markdown
#### For Slide Content Creation

<br>

The *same syntax* you use to create project   
**READMEs** and **Wikis** for your Git repos.

---

## Code Presenting
## Repo Source Files
<span style="font-size:0.6em; color:gray">Press Down key for examples.</span> |
<span style="font-size:0.6em; color:gray">See [GitPitch Wiki](https://github.com/gitpitch/gitpitch/wiki/Code-Presenting) for details.</span>

![Press Down Key](assets/down-arrow.png)

+++

#### Present Source Directly From Your Repo

<br>

Step through source code directly within your presentations.
*No more switching* back and forth between your slideshow and your IDE!

+++?code=src/elixir/monitor.ex&lang=elixir&title=Repo Source File: Elixir Snippets

@[11-14](Elixir module-attributes as constants)
@[22-28](Elixir with-statement for conciseness)
@[171-177](Elixir case-statement pattern matching)
@[179-185](Elixir pipe-mechanism for composing functions)=

---

## Code Presenting
## Static Source Blocks
<span style="font-size:0.6em; color:gray">Press Down key for examples.</span> |
<span style="font-size:0.6em; color:gray">See [GitPitch Wiki](https://github.com/gitpitch/gitpitch/wiki/Code-Presenting) for details.</span>

![Press Down Key](assets/down-arrow.png)

+++

#### Present Source Embedded In Your Presentation Markdown

<br>

Enjoy code syntax highlighting for dozens of languages powered by [highlight.js](tlhttps://highlightjs.org).

+++

Static Code Block: Python Snippets

```python
from time import localtime

activities = {8: 'Sleeping', 9: 'Commuting', 17: 'Working',
              18: 'Commuting', 20: 'Eating', 22: 'Resting' }

time_now = localtime()
hour = time_now.tm_hour

for activity_time in sorted(activities.keys()):
    if hour < activity_time:
        print activities[activity_time]
        break
else:
    print 'Unknown, AFK or sleeping!'
```

@[1](Python from..import statement)
@[3-4](Python dictionary initialization block)
@[6-7](Python working with time)
@[9-14](Python for..else statement)

---


## Image Slides
## [ Inline ]
<span style="font-size:0.6em; color:gray">Press Down key for examples.</span> |
<span style="font-size:0.6em; color:gray">See [GitPitch Wiki](https://github.com/gitpitch/gitpitch/wiki/Image-Slides) for details.</span>

![Press Down Key](assets/down-arrow.png)

+++

#### Make A Visual Statement

<br>

Use inline images to lend   
a *visual punch* to your slideshow presentations.


+++

<span style="color:gray; font-size:0.7em">Inline Image at <b>Absolute URL</b></span>

![Image-Absolute](https://d1z75bzl1vljy2.cloudfront.net/kitchen-sink/octocat-privateinvestocat.jpg)


<span style="color:gray; font-size: 0.5em;">the <b>Private Investocat</b> by [jeejkang](https://github.com/jeejkang)</span>


+++

<span style="color:gray; font-size:0.7em">Inline Image at GitHub Repo <b>Relative URL</b></span>

![Image-Absolute](assets/octocat-de-los-muertos.jpg)

<span style="color:gray; font-size:0.5em">the <b>Octocat-De-Los-Muertos</b> by [cameronmcefee](https://github.com/cameronmcefee)</span>


+++

<span style="color:gray; font-size:0.7em"><b>Animated GIFs</b> Work Too!</span>

![Image-Relative](https://d1z75bzl1vljy2.cloudfront.net/kitchen-sink/octocat-daftpunkocat.gif)

<span style="color:gray; font-size:0.5em">the <b>Daftpunktocat-Guy</b> by [jeejkang](https://github.com/jeejkang)</span>

---

## Image Slides
## [ Background ]
<span style="font-size:0.6em; color:gray">Press Down key for examples.</span> |
<span style="font-size:0.6em; color:gray">See [GitPitch Wiki](https://github.com/gitpitch/gitpitch/wiki/Image-Slides#background) for details.</span>

![Press Down Key](assets/down-arrow.png)

+++

#### Make A Bold Visual Statement

<br>

Use high-resolution background images   
for *maximum impact*.

+++?image=https://d1z75bzl1vljy2.cloudfront.net/kitchen-sink/victory.jpg

+++?image=https://d1z75bzl1vljy2.cloudfront.net/kitchen-sink/127.jpg


---

## Slide Fragments
<span style="font-size:0.6em; color:gray">Press Down key for examples.</span> |

<span style="font-size:0.6em; color:gray">See [GitPitch Wiki](https://github.com/gitpitch/gitpitch/wiki/Fragment-Slides) for details.</span>

![Press Down Key](assets/down-arrow.png)

+++

#### Reveal Slide Concepts Piecemeal

<br>

Step through slide content in sequence   
to *slowly reveal* the bigger picture.

+++

- Java
- Groovy |
- Kotlin |
- Scala  |
- The JVM rocks! |

+++

<table>
  <tr>
    <th>Firstname</th>
    <th>Lastname</th> 
    <th>Age</th>
  </tr>
  <tr>
    <td>Jill</td>
    <td>Smith</td>
    <td>25</td>
  </tr>
  <tr class="fragment">
    <td>Eve</td>
    <td>Jackson</td>
    <td>94</td>
  </tr>
  <tr class="fragment">
    <td>John</td>
    <td>Doe</td>
    <td>43</td>
  </tr>
</table>

---
## <span style="text-transform: none">PITCHME.yaml</span> Settings
<span style="font-size:0.6em; color:gray">Press Down key for examples.</span> |
<span style="font-size:0.6em; color:gray">See [GitPitch Wiki](https://github.com/gitpitch/gitpitch/wiki/Slideshow-Settings) for details.</span>

![Press Down Key](assets/down-arrow.png)

+++

#### Stamp Your Own Look and Feel

<br>

Set a default theme, custom logo, custom css, background image, and preferred code syntax highlighting style.

+++

#### Customize Slideshow Behavior

<br>

Enable auto-slide with custom slide intervals, presentation looping, and RTL flow.


---
## Slideshow Keyboard Controls
<span style="font-size:0.6em; color:gray">Press Down key for examples.</span> |
<span style="font-size:0.6em; color:gray">See [GitPitch Wiki](https://github.com/gitpitch/gitpitch/wiki/Slideshow-Fullscreen-Mode) for details.</span>

![Press Down Key](assets/down-arrow.png)

+++

#### Try Out These Great Features Now!

<br>

| Mode | On Key | Off Key |
| ---- | :------: | :--------: |
| Fullscreen | F |  Esc |
| Overview | O |  O |
| Blackout | B |  B |
| Help | ? |  Esc |


---

## GitPitch Social
<span style="font-size:0.6em; color:gray">Press Down key for examples.</span> |
<span style="font-size:0.6em; color:gray">See [GitPitch Wiki](https://github.com/gitpitch/gitpitch/wiki/Slideshow-GitHub-Badge) for details.</span>

![Press Down Key](assets/down-arrow.png)

+++

#### Slideshows Designed For Sharing

<br>

- View any slideshow at its public URL
- [Promote](https://github.com/gitpitch/gitpitch/wiki/Slideshow-GitHub-Badge) any slideshow using a GitHub badge
- [Embed](https://github.com/gitpitch/gitpitch/wiki/Slideshow-Embedding) any slideshow within a blog or website
- [Share](https://github.com/gitpitch/gitpitch/wiki/Slideshow-Sharing) any slideshow on Twitter, LinkedIn, etc
- [Print](https://github.com/gitpitch/gitpitch/wiki/Slideshow-Printing) any slideshow as a PDF document
- [Download and present](https://github.com/gitpitch/gitpitch/wiki/Slideshow-Offline) any slideshow offline

