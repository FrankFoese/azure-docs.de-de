---
title: Verhindern der Inhaltsübersetzung – Textübersetzungs-API
titlesuffix: Azure Cognitive Services
description: Verhindern Sie mit der Textübersetzungs-API die Übersetzung von Inhalten.
services: cognitive-services
author: Jann-Skotdal
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 11/20/2018
ms.author: v-jansko
ms.openlocfilehash: ca95f6a2bca10d9b8fcf263cfef6805ee936181a
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/07/2019
ms.locfileid: "55884697"
---
# <a name="how-to-prevent-translation-of-content-with-the-translator-text-api"></a>Verhindern der Inhaltsübersetzung mit der Textübersetzungs-API

Die Textübersetzungs-API ermöglicht es, Inhalte so zu kennzeichnen, dass sie nicht übersetzt werden. Sie können beispielsweise Code, einen Markennamen oder einzelne Wörter/Ausdrücke markieren, deren Übersetzung nicht sinnvoll ist. 

## <a name="methods-for-preventing-translation"></a>Methoden zum Verhindern der Übersetzung
1. Verwenden Sie als Escapezeichen ein Twitter-Tag wie @somethingtopassthrough oder #somethingtopassthrough. Entfernen Sie das Escapezeichen nach der Übersetzung.

2. Kennzeichnen Sie Ihren Inhalt mit `notranslate`.

   Beispiel:

   ```html
   <div class="notranslate">This will not be translated.</div>
   <div>This will be translated. </div>
   ```

3. Verwenden Sie das [dynamische Wörterbuch](dynamic-dictionary.md), um eine bestimmte Übersetzung vorzuschreiben.

4. Übergeben Sie die Zeichenfolge nicht zur Übersetzung an die Textübersetzungs-API.

5. Benutzerdefinierter Translator: Verwenden Sie ein [Wörterbuch in „Benutzerdefinierter Translator“](custom-translator/what-is-dictionary.md), um die Übersetzung eines Ausdrucks mit einer Wahrscheinlichkeit von 100% vorzuschreiben.


## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"]
> [Vermeiden einer Übersetzung in Ihrem Textübersetzungs-API-Aufruf](reference/v3-0-translate.md)
