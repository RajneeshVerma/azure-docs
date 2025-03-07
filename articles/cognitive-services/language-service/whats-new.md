---
title: What's new in Azure Cognitive Service for Language?
titleSuffix: Azure Cognitive Services
description: Find out about new releases and features for the Azure Cognitive Service for Language.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-service
ms.topic: overview
ms.date: 06/22/2022
ms.author: aahi
ms.custom: ignite-fall-2021, event-tier1-build-2022
---

# What's new in Azure Cognitive Service for Language?

Azure Cognitive Service for Language is updated on an ongoing basis. To stay up-to-date with recent developments, this article provides you with information about new releases and features.

## June 2022
* Python client library for [conversation summarization](summarization/quickstart.md?tabs=conversation-summarization&pivots=programming-language-python).

## May 2022

* PII detection for conversations.
* Rebranded Text Summarization to Document summarization.
* Conversation summarization is now available in public preview.

* The following features are now Generally Available (GA):
    * Custom text classification
    * Custom Named Entity Recognition (NER)
    * Conversational language understanding
    * Orchestration workflow

* The following updates for custom text classification, custom Named Entity Recognition (NER), conversational language understanding, and orchestration workflow:
    * Data splitting controls.
    * Ability to cancel training jobs.
    * Custom deployments can be named. You can have up to 10 deployments.
    * Ability to swap deployments.
    * Auto labeling (preview) for custom named entity recognition
    * Enterprise readiness support
    * Training modes for conversational language understanding
    * Updated service limits
    * Ability to use free (F0) tier for Language resources
    * Expanded regional availability
    * Updated model life cycle to add training configuration versions



## April 2022

* Fast Healthcare Interoperability Resources (FHIR) support is available in the [Language REST API preview](text-analytics-for-health/quickstart.md?pivots=rest-api&tabs=language) for Text Analytics for health.

## March 2022

* Expanded language support for:
  * [Custom text classification](custom-classification/language-support.md)
  * [Custom Named Entity Recognition (NER)](custom-named-entity-recognition/language-support.md)
  * [Conversational language understanding](conversational-language-understanding/language-support.md)

## February 2022

* Model improvements for latest model-version for [text summarization](summarization/overview.md)

* Model 2021-10-01 is Generally Available (GA) for [Sentiment Analysis and Opinion Mining](sentiment-opinion-mining/overview.md), featuring enhanced modeling for emojis and better accuracy across all supported languages.

* [Question Answering](question-answering/overview.md): Active learning v2 incorporates a better clustering logic providing improved accuracy of suggestions. It considers user actions when suggestions are accepted or rejected to avoid duplicate suggestions, and improve query suggestions.

## December 2021

* The version 3.1-preview.x REST endpoints and 5.1.0-beta.x client library have been retired. Please upgrade to the General Available version of the API(v3.1). If you're using the client libraries, use package version 5.1.0 or higher. See the [migration guide](./concepts/migrate-language-service-latest.md) for details.

## November 2021

* Based on ongoing customer feedback, we have increased the character limit per document for Text Analytics for health from 5,120 to 30,720.

* Azure Cognitive Service for Language release, with support for:

  * [Question Answering (now Generally Available)](question-answering/overview.md)
  * [Sentiment Analysis and opinion mining](sentiment-opinion-mining/overview.md)
  * [Key Phrase Extraction](key-phrase-extraction/overview.md)
  * [Named Entity Recognition (NER), Personally Identifying Information (PII)](named-entity-recognition/overview.md)
  * [Language Detection](language-detection/overview.md)
  * [Text Analytics for health](text-analytics-for-health/overview.md)
  * [Text summarization preview](summarization/overview.md)
  * [Custom Named Entity Recognition (Custom NER) preview](custom-named-entity-recognition/overview.md)
  * [Custom Text Classification preview](custom-classification/overview.md)
  * [Conversational Language Understanding preview](conversational-language-understanding/overview.md)

* Preview model version `2021-10-01-preview` for [Sentiment Analysis and Opinion mining](sentiment-opinion-mining/overview.md), which provides:

  * Improved prediction quality.
  * [Additional language support](sentiment-opinion-mining/language-support.md?tabs=sentiment-analysis) for the opinion mining feature.
  * For more information, see the [project z-code site](https://www.microsoft.com/research/project/project-zcode/).
  * To use this [model version](sentiment-opinion-mining/how-to/call-api.md#specify-the-sentiment-analysis-model), you must specify it in your API calls, using the model version parameter.

* SDK support for sending requests to custom models:

  * Custom Named Entity Recognition
  * Custom text classification
  * Custom language understanding

## Next steps

* See the [previous updates](./concepts/previous-updates.md) article for service updates not listed here. 
