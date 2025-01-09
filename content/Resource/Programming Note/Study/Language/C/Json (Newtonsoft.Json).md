---
tags:
  - Json
---


1. Newtonsoft.Json 사용법 [https://devstarsj.github.io/development/2016/06/11/CSharp.NewtonJSON/](https://devstarsj.github.io/development/2016/06/11/CSharp.NewtonJSON/)
2. Read JSON from a file
3. JObject o1 = JObject.Parse(File.ReadAllText(@"c:\videogames.json"));

// read JSON directly from a file

using (StreamReader file = File.OpenText(@"c:\videogames.json"))

using (JsonTextReader reader = new JsonTextReader(file))

{

JObject o2 = (JObject)JToken.ReadFrom(reader);

}