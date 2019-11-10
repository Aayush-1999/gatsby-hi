---
शीर्षक: सोर्स प्लगइन्स
typora-copy-images-to: ./
disableTableOfContents: true
---

> यह ट्यूटोरियल Gatsby के डेटा लेयर के बारे में एक श्रंखला का हिस्सा है। यहां जारी रखने से पहले सुनिश्चित करें कि आप [भाग 4] (/tutorial/part-four/) से गुजर चुके हैं।

## इस ट्यूटोरियल में क्या है?

इस ट्यूटोरियल में, आप GraphQL और सोर्स प्लगइन्स का उपयोग करके अपने Gatsby साइट में डेटा को कैसे पुल करना सीख सकते हैं। इससे पहले कि आप इन प्लगइन्स के बारे में जानें, आप यह जानना चाहेंगे कि GraphQL नामक एक चीज़ का उपयोग कैसे किया जाए, जो कि आपके क्वेरीज़ को सही ढंग से बनाने में मदद करता है।

## GraphiQL से परिचय

GraphQL, GraphQL एकीकृत विकास वातावरण (आईडीई) है। यह एक शक्तिशाली (और सभी के आसपास भयानक) उपकरण है जिसका उपयोग आप अक्सर Gatsby वेबसाइटों के निर्माण के दौरान करेंगे।


जब आपकी साइट का विकास सर्वर सामान्य रूप से चल रहा हो तो आप इसे एक्सेस कर सकते हैं
<http://localhost:8000/___graphql>.

<video controls="controls" autoplay="true" loop="true">
  <source type="video/mp4" src="/graphiql-explore.mp4"></source>
  <p>Your browser does not support the video element.</p>
</video>

अंतर्निहित 'site' "type" के आसपास प्रहार करें और देखें कि उस पर कौन से फ़ील्ड उपलब्ध हैं - जिसमें आप पहले से साइट किए गए `siteMetadata` ऑब्जेक्ट को शामिल करते हैं। GraphiQL खोलने की कोशिश करो और अपने डेटा के साथ खेलते हैं! स्वत: पूर्ण विंडो लाने के लिए <kbd> Ctrl + Space </ kbd> (या वैकल्पिक कीबोर्ड शॉर्टकट के रूप में <kbd> Shift + Space </ kbd> का उपयोग करें) और <kbd> Ctrl + </ kbd> को चलाने के लिए रेखांकन क्वेरी। आप शेष ट्यूटोरियल के माध्यम से बहुत अधिक GraphQL का उपयोग करेंगे।

## GraphiQL एक्सप्लोरर का उपयोग करना

GraphiQL एक्सप्लोरर आपको इन प्रश्नों को हाथ से टाइप करने की दोहराव प्रक्रिया के बिना उपलब्ध फ़ील्ड और इनपुट के माध्यम से क्लिक करके इंटरएक्टिव तरीके से पूर्ण क्वेरी बनाने में सक्षम बनाता है।

<EggheadEmbed
  lessonLink="https://egghead.io/lessons/gatsby-build-a-graphql-query-using-gatsby-s-graphiql-explorer"
  lessonTitle="Build a GraphQL Query using Gatsby’s GraphiQL Explorer"
/>

## Source plugins

Gatsby साइटों में डेटा कहीं से भी आ सकता है: एपीआई, डेटाबेस, सीएमएस, स्थानीय फाइलें, आदि।

स्रोत प्लगइन्स अपने स्रोत से डेटा प्राप्त करते हैं। जैसे फाइल सिस्टम स्रोत प्लगइन फाइल सिस्टम से डेटा प्राप्त करना जानता है। वर्डप्रेस प्लगइन वर्डप्रेस एपीआई से डेटा प्राप्त करना जानता है।

[`Gatsby-source-filesystem`] (/packages/gatsby-source-filesystem/) ऐड करें और जानें कि यह कैसे काम करता है।

सबसे पहले, प्रोजेक्ट की रुट में प्लगइन इंस्टॉल करें:

```shell
npm install --save gatsby-source-filesystem
```

फिर इसे अपने `gatsby-config.js` में ऐड करें:

```javascript:title=gatsby-config.js
module.exports = {
  siteMetadata: {
    title: `Pandas Eating Lots`,
  },
  plugins: [
    // highlight-start
    {
      resolve: `gatsby-source-filesystem`,
      options: {
        name: `src`,
        path: `${__dirname}/src/`,
      },
    },
    // highlight-end
    `gatsby-plugin-emotion`,
    {
      resolve: `gatsby-plugin-typography`,
      options: {
        pathToConfigModule: `src/utils/typography`,
      },
    },
  ],
}
```

सहेजें और gatsby विकास सर्वर को पुनरारंभ करें। इसके बाद फिर से GraphiQL को खोलें।

एक्सप्लोरर फलक में, आप `allFile` और` file` चयनों के रूप में उपलब्ध देखेंगे:

![graphiql-filesystem](graphiql-filesystem.png)

`AllFile` ड्रॉपडाउन पर क्लिक करें। क्वेरी क्षेत्र में `allFile 'के बाद अपना कर्सर रखें, और फिर टाइप करें <kbd> Ctrl + Enter </ kbd>। यह प्रत्येक फ़ाइल के `आईडी` के लिए एक क्वेरी भर देगा। क्वेरी चलाने के लिए "Play" दबाएं:

![filesystem-query](filesystem-query.png)

एक्सप्लोरर फलक में, `आईडी` फ़ील्ड को स्वचालित रूप से चुना गया है। फ़ील्ड के संबंधित चेकबॉक्स की जाँच करके अधिक फ़ील्ड के लिए चयन करें। नए फ़ील्ड के साथ क्वेरी चलाने के लिए "Play" दबाएं:

![filesystem-explorer-options](filesystem-explorer-options.png)

वैकल्पिक रूप से, आप स्वतः पूर्ण शॉर्टकट (<kbd> Ctrl + Space </ kbd>) का उपयोग करके फ़ील्ड जोड़ सकते हैं। यह `File` नोड्स पर क्वेरी करने योग्य फ़ील्ड दिखाएगा।

![filesystem-autocomplete](filesystem-autocomplete.png)

अपनी क्वेरी में कई फ़ील्ड जोड़ने का प्रयास करें, <kbd> Ctrl + Enter </ kbd> दबाएं
हर बार क्वेरी को फिर से चलाने के लिए। आपको अपडेट किए गए क्वेरी परिणाम दिखाई देंगे:

![allfile-query](allfile-query.png)

इसका परिणाम 'File` "nodes" का एक सरणी है (नोड एक में एक वस्तु के लिए एक फैंसी नाम है
"graph")। प्रत्येक 'File' नोड ऑब्जेक्ट में आपके द्वारा क्वेर किए गए फ़ील्ड हैं।

## एक GraphQL क्वेरी के साथ एक पृष्ठ बनाएँ

Gatsby के साथ नए पृष्ठों का निर्माण अक्सर GraphiQL में शुरू होता है। आप पहले स्केच करें
GraphiQL में खेलने के द्वारा डेटा क्वेरी तो एक प्रतिक्रिया पृष्ठ घटक के लिए इसे कॉपी
UI का निर्माण शुरू करने के लिए।

चलो यह करके देखें।

`Src / Pages / my-files.js` पर एक नई फ़ाइल बनाएँ, जिसमें 'allFile` GraphQL क्वेरी आपके लिए है
बनाया था:

```jsx:title=src/pages/my-files.js
import React from "react"
import { graphql } from "gatsby"
import Layout from "../components/layout"

export default ({ data }) => {
  console.log(data) // highlight-line
  return (
    <Layout>
      <div>Hello world</div>
    </Layout>
  )
}

export const query = graphql`
  query {
    allFile {
      edges {
        node {
          relativePath
          prettySize
          extension
          birthTime(fromNow: true)
        }
      }
    }
  }
`
```

`console.log(data)` रेखा के ऊपर प्रकाश डाला गया है। यह अक्सर मददगार होता है
GraphQL क्वेरी से आपके द्वारा प्राप्त किए जा रहे डेटा को सांत्वना देने के लिए एक नया घटक बनाना
इसलिए आप UI बनाते समय अपने ब्राउज़र कंसोल में डेटा का पता लगा सकते हैं।

यदि आप `/my-files/` पर नए पृष्ठ पर जाते हैं और अपने ब्राउज़र कंसोल को खोलते हैं
आप कुछ इस तरह देखेंगे:


![data-in-console](data-in-console.png)

डेटा का आकार GraphQL क्वेरी के आकार से मेल खाता है।

फ़ाइल डेटा को प्रिंट करने के लिए अपने घटक में कुछ कोड जोड़ें।


```jsx:title=src/pages/my-files.js
import React from "react"
import { graphql } from "gatsby"
import Layout from "../components/layout"

export default ({ data }) => {
  console.log(data)
  return (
    <Layout>
      {/* highlight-start */}
      <div>
        <h1>My Site's Files</h1>
        <table>
          <thead>
            <tr>
              <th>relativePath</th>
              <th>prettySize</th>
              <th>extension</th>
              <th>birthTime</th>
            </tr>
          </thead>
          <tbody>
            {data.allFile.edges.map(({ node }, index) => (
              <tr key={index}>
                <td>{node.relativePath}</td>
                <td>{node.prettySize}</td>
                <td>{node.extension}</td>
                <td>{node.birthTime}</td>
              </tr>
            ))}
          </tbody>
        </table>
      </div>
      {/* highlight-end */}
    </Layout>
  )
}

export const query = graphql`
  query {
    allFile {
      edges {
        node {
          relativePath
          prettySize
          extension
          birthTime(fromNow: true)
        }
      }
    }
  }
`
```

और अब [http://localhost:8000/my-files](http://localhost:8000/my-files)… पर जाएँ 😲

![my-files-page](my-files-page.png)

## आगे क्या आ रहा है?

अब आप जान गए हैं कि स्रोत प्लगइन data_into_ Gatsby का डेटा सिस्टम कैसे लाते हैं। अगले ट्यूटोरियल में, आप सीखेंगे कि कैसे ट्रांसफॉर्मर प्लगइन्स _transform_ स्रोत प्लगइन्स द्वारा लाया गया कच्चा माल है। स्रोत प्लगइन्स और ट्रांसफ़ॉर्मर प्लगइन्स का संयोजन सभी डेटा सोर्सिंग और डेटा ट्रांसफ़ॉर्मेशन को हैंडल कर सकता है जो आपको एक Gatsby साइट के निर्माण के समय की आवश्यकता हो सकती है।(/tutorial/part-six/) में ट्रांसफॉर्मर प्लगइन्स के बारे में जानें।
