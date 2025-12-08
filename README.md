# Extension Master – Usage Guide 💡

This document shows **how to use all extension helpers** included in the project.  
They are grouped by **type** (Transform, GameObject, Vector, etc.) and each section shows:

- ✅ **What it does**
- 🧩 **Example usage**
- 🧠 **What it replaces**

> ⚠️ All code samples assume you have the proper `using` statements for your extension namespaces, e.g.:
> ```csharp
> using ExtensionMasterTool;
> ```

---


# Transform Extensions – Usage Examples

## I. Reset & Basic Setters

### 1. ResetLocalPosition

| Code Example                        | Normal Code                                 | Description                                                                                 |
| :---------------------------------- | :------------------------------------------ | :------------------------------------------------------------------------------------------ |
| `childToMove.ResetLocalPosition();` | `childToMove.localPosition = Vector3.zero;` | Resets the Transform’s **local position** to `(0,0,0)` without affecting rotation or scale. |

### 2. SetLocalScale

| Code Example                        | Normal Code                                                             | Description                                                           |
| :---------------------------------- | :---------------------------------------------------------------------- | :-------------------------------------------------------------------- |
| `childToMove.SetLocalScale(y: 2f);` | `var s = childToMove.localScale; s.y = 2f; childToMove.localScale = s;` | Updates one or more **local scale axes** while preserving the others. |

### 3. SetLocalRotationEuler

| Code Example                                 | Normal Code                                                                          | Description                                                               |
| :------------------------------------------- | :----------------------------------------------------------------------------------- | :------------------------------------------------------------------------ |
| `childToMove.SetLocalRotationEuler(z: 90f);` | `var e = childToMove.localEulerAngles; e.z = 90f; childToMove.localEulerAngles = e;` | Sets specific **Euler rotation axes**, keeping the remaining axes intact. |

### 4. ResetRotation

| Code Example                   | Normal Code                                        | Description                                  |
| :----------------------------- | :------------------------------------------------- | :------------------------------------------- |
| `childToMove.ResetRotation();` | `childToMove.localRotation = Quaternion.identity;` | Resets local rotation to identity `(0,0,0)`. |

### 5. ResetScale

| Code Example                | Normal Code                             | Description                      |
| :-------------------------- | :-------------------------------------- | :------------------------------- |
| `childToMove.ResetScale();` | `childToMove.localScale = Vector3.one;` | Resets local scale to `(1,1,1)`. |

### 6. SetPosition

| Code Example                       | Normal Code                                                          | Description                                                 |
| :--------------------------------- | :------------------------------------------------------------------- | :---------------------------------------------------------- |
| `childToMove.SetPosition(x: 10f);` | `var p = childToMove.position; p.x = 10f; childToMove.position = p;` | Sets specific **world position axes**, preserving the rest. |

### 7. ResetLocal

| Code Example                | Normal Code                                                    | Description                                                   |
| :-------------------------- | :------------------------------------------------------------- | :------------------------------------------------------------ |
| `childToMove.ResetLocal();` | `localPosition = 0; localRotation = identity; localScale = 1;` | Fully resets local position, rotation, and scale in one call. |

### 8. ResetWorld

| Code Example                | Normal Code                                                | Description                         |
| :-------------------------- | :--------------------------------------------------------- | :---------------------------------- |
| `childToMove.ResetWorld();` | `position = Vector3.zero; rotation = Quaternion.identity;` | Resets world position and rotation. |

## II. Hierarchy Management

### 9. SetParentAndReset

| Code Example                                   | Normal Code                                                | Description                                                   |
| :--------------------------------------------- | :--------------------------------------------------------- | :------------------------------------------------------------ |
| `childToMove.SetParentAndReset(targetParent);` | `SetParent(targetParent); Reset local transform manually.` | Assigns a new parent and resets local transforms immediately. |

### 10. GetChildren

| Code Example                             | Normal Code                                        | Description                                  |
| :--------------------------------------- | :------------------------------------------------- | :------------------------------------------- |
| `var list = targetParent.GetChildren();` | `Manual loop adding each child to List<Transform>` | Returns all direct children as a clean List. |

### 11. GetSiblingIndexChecked

| Code Example                                        | Normal Code                                           | Description                                                   |
| :-------------------------------------------------- | :---------------------------------------------------- | :------------------------------------------------------------ |
| `int index = childToMove.GetSiblingIndexChecked();` | `childToMove.parent != null ? GetSiblingIndex() : -1` | Returns safe sibling index; returns `-1` if no parent exists. |

# GameObject Extensions – Usage Examples

## I. Component Management

### 1. GetOrAddComponent<T>

| Code Example                                                   | Normal Code                                                                                                                         | Description                                              |
| :------------------------------------------------------------- | :---------------------------------------------------------------------------------------------------------------------------------- | :------------------------------------------------------- |
| `testCompRef = rootObject.GetOrAddComponent<TestComponent>();` | `TestComponent comp = rootObject.GetComponent<TestComponent>(); if (comp == null) comp = rootObject.AddComponent<TestComponent>();` | Safely gets an existing component or adds it if missing. |

### 2. HasComponent<T>

| Code Example                                               | Normal Code                                        | Description                                              |
| :--------------------------------------------------------- | :------------------------------------------------- | :------------------------------------------------------- |
| `bool hasComp = rootObject.HasComponent<TestComponent>();` | `rootObject.GetComponent<TestComponent>() != null` | Quickly checks if a GameObject has a specific component. |

### 3. RemoveComponent<T>

| Code Example                                   | Normal Code                                                                                                | Description                                                         |
| :--------------------------------------------- | :--------------------------------------------------------------------------------------------------------- | :------------------------------------------------------------------ |
| `rootObject.RemoveComponent<TestComponent>();` | `TestComponent comp = rootObject.GetComponent<TestComponent>(); if (comp != null) DestroyImmediate(comp);` | Safely removes a component from a GameObject (Editor/Runtime safe). |

## II. Activation and State

### 4. ToggleActive

| Code Example                 | Normal Code                                     | Description                            |
| :--------------------------- | :---------------------------------------------- | :------------------------------------- |
| `rootObject.ToggleActive();` | `rootObject.SetActive(!rootObject.activeSelf);` | Inverts the GameObject's active state. |

### 5. IsActiveAndEnabled

| Code Example                       | Normal Code                    | Description                                            |
| :--------------------------------- | :----------------------------- | :----------------------------------------------------- |
| `rootObject.IsActiveAndEnabled();` | `rootObject.activeInHierarchy` | Checks if the object is fully active in the hierarchy. |

### 6. SetChildrenActive

| Code Example                          | Normal Code                                                                   | Description                                                      |
| :------------------------------------ | :---------------------------------------------------------------------------- | :--------------------------------------------------------------- |
| `rootObject.SetChildrenActive(true);` | `foreach (Transform t in rootObject.transform) t.gameObject.SetActive(true);` | Sets active state for all immediate children based on a boolean. |

### 7. SetChildActiveAt

| Code Example                             | Normal Code                                                                                              | Description                                               |
| :--------------------------------------- | :------------------------------------------------------------------------------------------------------- | :-------------------------------------------------------- |
| `rootObject.SetChildActiveAt(0, false);` | `if (0 < rootObject.transform.childCount) rootObject.transform.GetChild(0).gameObject.SetActive(false);` | Sets active state for a child at a specific index safely. |

## III. Hierarchy, Tag & Layer Management

### 8. InstantiateAsChild

| Code Example                                                              | Normal Code                                                                                           | Description                                        |
| :------------------------------------------------------------------------ | :---------------------------------------------------------------------------------------------------- | :------------------------------------------------- |
| `GameObject clone = rootObject.InstantiateAsChild(rootObject.transform);` | `GameObject clone = Instantiate(rootObject); clone.transform.SetParent(rootObject.transform, false);` | Clones the object and parents it in a single call. |

### 9. Rename

| Code Example                | Normal Code                | Description                            |
| :-------------------------- | :------------------------- | :------------------------------------- |
| `clone.Rename(CLONE_NAME);` | `clone.name = CLONE_NAME;` | Changes the GameObject's name cleanly. |

### 10. SetLayerRecursive

| Code Example                       | Normal Code                                                                                        | Description                                             |
| :--------------------------------- | :------------------------------------------------------------------------------------------------- | :------------------------------------------------------ |
| `rootObject.SetLayerRecursive(1);` | `foreach (Transform t in rootObject.GetComponentsInChildren<Transform>()) t.gameObject.layer = 1;` | Sets layer for the object and all children recursively. |

### 11. SetTagRecursive

| Code Example                             | Normal Code                                                                                              | Description                                           |
| :--------------------------------------- | :------------------------------------------------------------------------------------------------------- | :---------------------------------------------------- |
| `rootObject.SetTagRecursive("Respawn");` | `foreach (Transform t in rootObject.GetComponentsInChildren<Transform>()) t.gameObject.tag = "Respawn";` | Sets tag for the object and all children recursively. |

### 12. DestroyChildAt

| Code Example                    | Normal Code                                                                                      | Description                                                     |
| :------------------------------ | :----------------------------------------------------------------------------------------------- | :-------------------------------------------------------------- |
| `rootObject.DestroyChildAt(0);` | `if (0 < rootObject.transform.childCount) Destroy(rootObject.transform.GetChild(0).gameObject);` | Safely destroys a child by index after performing bounds check. |

# Vector Extensions – Usage Examples

## I. Axis Manipulation (With Setter)

### 1a. With()

| Code Example                                            | Normal Code                                                           | Description                                            |
| :------------------------------------------------------ | :-------------------------------------------------------------------- | :----------------------------------------------------- |
| `Vector3 newVectorWith = initialPosition.With(y: 20f);` | `Vector3 newVectorNormal = initialPosition; newVectorNormal.y = 20f;` | Sets the Y value of a vector while preserving X and Z. |

### 1b. SetX()

| Code Example                                        | Normal Code                                    | Description                                            |
| :-------------------------------------------------- | :--------------------------------------------- | :----------------------------------------------------- |
| `Vector3 newVectorSetX = initialPosition.SetX(1f);` | `Vector3 temp = initialPosition; temp.x = 1f;` | Sets the X value of a vector while preserving Y and Z. |

## II. Fuzzy Comparison (IsApproximately)

### 2a. IsApproximately()

| Code Example                                                              | Normal Code                                                                                                                  | Description                                                                                                 |
| :------------------------------------------------------------------------ | :--------------------------------------------------------------------------------------------------------------------------- | :---------------------------------------------------------------------------------------------------------- |
| `bool isClose = initialPosition.IsApproximately(targetPosition, 0.001f);` | `float sqrDistance = (initialPosition - targetPosition).sqrMagnitude; bool isCloseNormal = sqrDistance < (0.001f * 0.001f);` | Checks if two vectors are approximately equal within a tolerance, ignoring floating-point precision errors. |

## III. Math Helpers

### 4. DirectionTo()

| Code Example                                           | Normal Code                         | Description                                                  |
| :----------------------------------------------------- | :---------------------------------- | :----------------------------------------------------------- |
| `Vector3 direction = playerPos.DirectionTo(enemyPos);` | `(enemyPos - playerPos).normalized` | Returns a normalized direction vector from source to target. |

### 5. IsZero()

| Code Example                               | Normal Code                                     | Description                                                              |
| :----------------------------------------- | :---------------------------------------------- | :----------------------------------------------------------------------- |
| `bool isZeroResult = zeroVector.IsZero();` | `zeroVector.sqrMagnitude < (0.0001f * 0.0001f)` | Checks if the vector’s magnitude is near zero (floating point tolerant). |

### 6. Abs()

| Code Example                                     | Normal Code                                                   | Description                                               |
| :----------------------------------------------- | :------------------------------------------------------------ | :-------------------------------------------------------- |
| `Vector3 absoluteVector = negativeVector.Abs();` | `new Vector3(Mathf.Abs(v.x), Mathf.Abs(v.y), Mathf.Abs(v.z))` | Returns a vector with absolute values for all components. |

## IV. Vector2 Conversion

### 7. ToVector3()

| Code Example                   | Normal Code                   | Description                                    |
| :----------------------------- | :---------------------------- | :--------------------------------------------- |
| `Vector3 v3 = v2.ToVector3();` | `new Vector3(v2.x, v2.y, 0f)` | Converts a Vector2 to Vector3, setting Z to 0. |

# Number Extensions – Usage Examples (Float & Int)

## I. Float Checks & Comparison

### 1a. IsApproximately

| Code Example                                                           | Normal Code                                          | Description                                                             |
| :--------------------------------------------------------------------- | :--------------------------------------------------- | :---------------------------------------------------------------------- |
| `bool isClose = TEST_FLOAT.IsApproximately(COMPARISON_FLOAT, 0.001f);` | `Mathf.Abs(TEST_FLOAT - COMPARISON_FLOAT) <= 0.001f` | Checks if two floats are approximately equal within a custom tolerance. |

### 1b. IsBetween (Float)

| Code Example                                | Normal Code            | Description                                                |
| :------------------------------------------ | :--------------------- | :--------------------------------------------------------- |
| `bool inRange = 15.5f.IsBetween(10f, 20f);` | `f >= 10f && f <= 20f` | Checks if a float is within a specified range (inclusive). |

### 1c. IsNegative

| Code Example            | Normal Code | Description                        |
| :---------------------- | :---------- | :--------------------------------- |
| `(-5.0f).IsNegative();` | `f < 0`     | Determines if a float is negative. |

### 1d. ToNormalizedPercentage

| Code Example                                           | Normal Code  | Description                                                       |
| :----------------------------------------------------- | :----------- | :---------------------------------------------------------------- |
| `float normalized = 75f.ToNormalizedPercentage(300f);` | `75f / 300f` | Converts a float to a 0-1 normalized percentage of a total value. |

## II. Float Formatting & Rounding

### 2a. ToTimeFormat

| Code Example                                           | Normal Code                                                                                                                                               | Description                                               |
| :----------------------------------------------------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------- | :-------------------------------------------------------- |
| `string timeStr = TARGET_TIME_SECONDS.ToTimeFormat();` | `TimeSpan t = TimeSpan.FromSeconds(TARGET_TIME_SECONDS); string timeStr = string.Format("{0}:{1:00}.{2:00}", t.Minutes, t.Seconds, t.Milliseconds / 10);` | Formats seconds as a clean time string (e.g., "m:ss.ff"). |

### 2b. RoundTo

| Code Example                           | Normal Code                                       | Description                                               |
| :------------------------------------- | :------------------------------------------------ | :-------------------------------------------------------- |
| `float rounded = 12.3456f.RoundTo(2);` | `float rounded = (float)Math.Round(12.3456f, 2);` | Rounds a float to the specified number of decimal places. |

### 2c. ToDegrees

| Code Example                        | Normal Code                | Description                         |
| :---------------------------------- | :------------------------- | :---------------------------------- |
| `float deg = Mathf.PI.ToDegrees();` | `Mathf.PI * Mathf.Rad2Deg` | Converts a radian value to degrees. |

## III. Int Checks

### 3a. IsEven / IsOdd

| Code Example        | Normal Code  | Description                   |
| :------------------ | :----------- | :---------------------------- |
| `TEST_INT.IsEven()` | `i % 2 == 0` | Checks if an integer is even. |
| `43.IsOdd()`        | `i % 2 != 0` | Checks if an integer is odd.  |

### 3b. IsBetween (Int)

| Code Example                      | Normal Code           | Description                                                   |
| :-------------------------------- | :-------------------- | :------------------------------------------------------------ |
| `NEGATIVE_INT.IsBetween(-10, -1)` | `i >= -10 && i <= -1` | Checks if an integer is within a specified range (inclusive). |

### 3c. IsPositive

| Code Example                | Normal Code | Description                                             |
| :-------------------------- | :---------- | :------------------------------------------------------ |
| `NEGATIVE_INT.IsPositive()` | `i > 0`     | Determines if an integer is strictly greater than zero. |

# Collection Extensions – Usage Examples (List & Array)

## I. Safe Access & Checking

### 1a. IsNullOrEmpty

| Code Example            | Normal Code     | Description |                    |                                                        |
| :---------------------- | :-------------- | :---------- | ------------------ | ------------------------------------------------------ |
| `names.IsNullOrEmpty()` | `(names == null |             | names.Count == 0)` | Checks if the collection is null or has zero elements. |

### 1b. GetSafe<T>

| Code Example                              | Normal Code                                                                        | Description                                                                  |
| :---------------------------------------- | :--------------------------------------------------------------------------------- | :--------------------------------------------------------------------------- |
| `string safeElement = names.GetSafe(99);` | `if (index >= 0 && index < names.Count) return names[index]; else return default;` | Safely gets an element at a given index, returning default if out of bounds. |

### 1c. TryGet<T>

| Code Example                                         | Normal Code                                                                              | Description                                                                    |
| :--------------------------------------------------- | :--------------------------------------------------------------------------------------- | :----------------------------------------------------------------------------- |
| `if (names.TryGet(1, out string foundName)) { ... }` | `if (1 >= 0 && 1 < names.Count) { foundName = names[1]; } else { foundName = default; }` | Tries to get an element, returning true/false and the value via out parameter. |

## II. Randomness & Ordering

### 2a. RandomElement (List)

| Code Example                                 | Normal Code                                                                   | Description                             |
| :------------------------------------------- | :---------------------------------------------------------------------------- | :-------------------------------------- |
| `string randomName = names.RandomElement();` | `int index = Random.Range(0, names.Count); string randomName = names[index];` | Returns a random element from the list. |

### 2b. RandomElement (Array)

| Code Example                                    | Normal Code                                                                         | Description                             |
| :---------------------------------------------- | :---------------------------------------------------------------------------------- | :-------------------------------------- |
| `string randomEnemy = enemies.RandomElement();` | `int index = Random.Range(0, enemies.Length); string randomEnemy = enemies[index];` | Returns a random element from an array. |

### 2c. Shuffle

| Code Example       | Normal Code                                           | Description                           |
| :----------------- | :---------------------------------------------------- | :------------------------------------ |
| `names.Shuffle();` | `// Implement Fisher-Yates algorithm or LINQ shuffle` | Randomly reorders elements of a list. |

### 2d. RemoveRandom

| Code Example                             | Normal Code                                                                                       | Description                              |
| :--------------------------------------- | :------------------------------------------------------------------------------------------------ | :--------------------------------------- |
| `string removed = names.RemoveRandom();` | `int index = Random.Range(0, names.Count); string removed = names[index]; names.RemoveAt(index);` | Removes a random element and returns it. |

## III. Bulk Operations

### 3a. AddRangeUnique

| Code Example                                         | Normal Code                                                                      | Description                                                                         |
| :--------------------------------------------------- | :------------------------------------------------------------------------------- | :---------------------------------------------------------------------------------- |
| `int addedCount = names.AddRangeUnique(itemsToAdd);` | `foreach(var item in itemsToAdd) { if(!names.Contains(item)) names.Add(item); }` | Adds items to the list only if they don't already exist, returning the count added. |

### 3b. RemoveAll

| Code Example                                            | Normal Code                                                  | Description                                                    |
| :------------------------------------------------------ | :----------------------------------------------------------- | :------------------------------------------------------------- |
| `names.RemoveAll(new List<string> { "Alice", "Bob" });` | `foreach(var item in itemsToRemove) { names.Remove(item); }` | Removes all items present in another collection from the list. |

# String Extensions – Usage Examples

## I. Validation and Checking

### 1a. IsNullOrEmpty

| Code Example                 | Normal Code                        | Description                            |
| :--------------------------- | :--------------------------------- | :------------------------------------- |
| `nullString.IsNullOrEmpty()` | `string.IsNullOrEmpty(nullString)` | Checks if the string is null or empty. |

### 1b. IsNullOrWhiteSpace

| Code Example                            | Normal Code                                   | Description                                                          |
| :-------------------------------------- | :-------------------------------------------- | :------------------------------------------------------------------- |
| `whiteSpaceString.IsNullOrWhiteSpace()` | `string.IsNullOrWhiteSpace(whiteSpaceString)` | Checks if the string is null, empty, or consists only of whitespace. |

### 1c. ContainsAny

| Code Example                                   | Normal Code                                                                         | Description                                                    |
| :--------------------------------------------- | :---------------------------------------------------------------------------------- | :------------------------------------------------------------- |
| `containsTestString.ContainsAny(searchValues)` | `foreach(var s in searchValues){ if(containsTestString.Contains(s)) return true; }` | Checks if the string contains any of the specified substrings. |

### 1d. IsNumeric

| Code Example                | Normal Code                            | Description                                     |
| :-------------------------- | :------------------------------------- | :---------------------------------------------- |
| `numericString.IsNumeric()` | `float.TryParse(numericString, out _)` | Checks if the string can be parsed as a number. |

## II. Case Conversion and Length

### 2a. ToUpper

| Code Example               | Normal Code                | Description                           |
| :------------------------- | :------------------------- | :------------------------------------ |
| `caseTestString.ToUpper()` | `caseTestString.ToUpper()` | Converts all characters to uppercase. |

### 2b. ToLower

| Code Example               | Normal Code                | Description                           |
| :------------------------- | :------------------------- | :------------------------------------ |
| `caseTestString.ToLower()` | `caseTestString.ToLower()` | Converts all characters to lowercase. |

### 2c. GetLength

| Code Example                 | Normal Code             | Description                       |
| :--------------------------- | :---------------------- | :-------------------------------- |
| `caseTestString.GetLength()` | `caseTestString.Length` | Returns the length of the string. |

## III. Manipulation and Cleaning

### 3a. RemoveChar

| Code Example                                 | Normal Code                                                        | Description                                                     |
| :------------------------------------------- | :----------------------------------------------------------------- | :-------------------------------------------------------------- |
| `charRemovalString.RemoveChar(charToRemove)` | `charRemovalString.Replace(charToRemove.ToString(), string.Empty)` | Removes all instances of a specified character from the string. |

### 3b. Cut

| Code Example                 | Normal Code                                                             | Description                                                 |
| :--------------------------- | :---------------------------------------------------------------------- | :---------------------------------------------------------- |
| `longContent.Cut(maxLength)` | `if(str.Length > maxLength) str = str.Substring(0, maxLength) + "...";` | Truncates the string to a maximum length and adds ellipsis. |

### 3c. RemoveWhitespace

| Code Example                     | Normal Code                                                | Description                                                 |
| :------------------------------- | :--------------------------------------------------------- | :---------------------------------------------------------- |
| `"a b\tc\nd".RemoveWhitespace()` | `str.Replace(" ", "").Replace("\t", "").Replace("\n", "")` | Removes all whitespace, tabs, and newlines from the string. |

## IV. Conversion and Splitting

### 4a. ToCharArray / ToCharList

| Code Example                                                     | Normal Code                                                                       | Description                                            |
| :--------------------------------------------------------------- | :-------------------------------------------------------------------------------- | :----------------------------------------------------- |
| `splittingString.ToCharArray()` / `splittingString.ToCharList()` | `splittingString.ToCharArray()` / `new List<char>(splittingString.ToCharArray())` | Converts a string to an array or a list of characters. |

### 4b. ToWordArray / ToWordList

| Code Example                   | Normal Code                                                                        | Description                                       |
| :----------------------------- | :--------------------------------------------------------------------------------- | :------------------------------------------------ |
| `splittingString.ToWordList()` | `splittingString.Split(new char[] { ' ' }, StringSplitOptions.RemoveEmptyEntries)` | Splits the string into words based on whitespace. |
| Code Example | Description | Expected Console Output |
| :--- | :--- | :--- |
| `string desc = currentDifficulty.GetDescription();` | Reads Description attribute; falls back to Enum name if none exists. | `[LOG] Description for Difficulty.Medium: Medium` |
| `string name = currentDifficulty.GetName();` | Returns the Enum name as string. | `[LOG] Enum's String Name: Medium` |
| `Difficulty nextDiff = currentDifficulty.Next();` | Moves to next Enum value, wraps to start if at end. | `[LOG] Current (Medium) after Next(): Hard` |
| `Difficulty previousDiff = Difficulty.Easy.Previous();` | Moves to previous Enum value, wraps to end if at start. | `[LOG] Easy after Previous(): Nightmare` |
| `Difficulty[] allValues = EnumExtensions.GetValues<Difficulty>();` | Retrieves all defined Enum values as an array. | `[LOG] All Difficulty values: Easy, Medium, Hard, Nightmare` |
| `PlayerAbilities newAbilities = currentAbilities.SetFlag(targetFlag, enableFlag);` | Adds or removes a specified flag based on boolean. | `[LOG] Does the new set have Shield? -> True`<br>`[LOG] New Ability Set: DoubleJump, Dash, Shield` |
| `PlayerAbilities removeAbilities = newAbilities.SetFlag(PlayerAbilities.DoubleJump, false);` | Removes the DoubleJump flag explicitly. | `[LOG] Was DoubleJump successfully removed? -> True`<br>`[LOG] Final Ability Set: Dash, Shield` |

#Text – Usage Examples (Text & TextMeshPro)
| Code Example | Description | Expected Console Output |
| :--- | :--- | :--- |
| `targetTextComponent.SetTextAndColor("Status: Initialized", newTargetColor);` | Sets both text and color in one call. | `[LOG] SetTextAndColor applied. Text is now 'Status: Initialized' and color is RGBA(0,1,1,1)` |
| `targetTextComponent.SetAlpha(0.5f);` | Changes only the alpha channel of the color. | `[LOG] SetAlpha applied. Alpha is now 0.5` |
| `targetTextComponent.SetTextSafe("Status: Running Safely");` | Safely sets text ensuring component is not null. | *(No console output, sets text internally)* |
| `cleanTestText.IsTextSet(); cleanTestText.ClearText();` | Checks if text exists and clears it. | `[LOG] Is cleanTestText set? -> True. It is now cleared.` |
| `targetTextComponent.ToggleVisibility();` | Toggles alpha between 0 and 1 instantly. | `[LOG] ToggleVisibility called. Alpha is now 0` |
| `string basicTags = baseContent.ToBold().ToItalic();` | Wraps string with <b> and <i> tags. | `[LOG] Basic Tags: <i><b>Extension Master Tool</b></i>` |
| `string colorTag = baseContent.Colorize(tagColor);` | Wraps string in color tags for TMP. | `[LOG] Color Tag: <color=#FF0000>Extension Master Tool</color>` |
| `string finalFormattedText = "Score: ".Colorize(Color.white) + baseContent.ToBold().Colorize(tagColor).Size(targetSize); outputTextComponent.text = finalFormattedText;` | Combines multiple tags: color, bold, size. | `[LOG] Final Formatted Output Set to: <color=#FFFFFF>Score: </color><size=150><color=#FF0000><b>Extension Master Tool</b></color></size>` |
| `StartCoroutine(outputTextComponent.FadeToAlpha(1f, fadeDuration));` | Fades text alpha from current to 1.0 over duration. | `[LOG] Starting FadeToAlpha to alpha 1.0 over 1.5 seconds.` |
| `targetTextComponent.SetTextFormat("Active Components: {0} / {1}", 2, 3);` | Formats text using string.Format syntax. | *(Text updates internally, no console output)* |

# Image Extensions – Usage Examples 
| Code Example | Description | Expected Console Output |
| :--- | :--- | :--- |
| `targetImage.SetColor(imageColor);` | Sets Image component color directly. | `[LOG] SetColor applied: Image color is now RGBA(1,1,0,1)` |
| `targetImage.SetAlpha(0.5f);` | Adjusts alpha channel only. | `[LOG] SetAlpha applied: Alpha is now 0.5` |
| `targetImage.SetSprite(newSprite);` | Changes the image sprite if provided. | `[LOG] SetSprite applied: Image sprite updated.` |
| `targetImage.ToggleVisibility();` | Toggles alpha between 0 and 1. | `[LOG] ToggleVisibility called. Alpha is now 0.0` |
| `targetButton.SetInteractable(true);` | Enables or disables the button interactability. | `[LOG] SetInteractable(true) applied. Button state: True` |
| `targetButton.AddListener(() => OnButtonClicked("Extension Master Button"));` | Adds an onClick listener directly. | `[LOG] AddListener applied: Check console when you click the button in Play Mode.` |
| `targetButton.ToggleInteractable();` | Toggles interactable state. | `[LOG] ToggleInteractable called. Button state: False` |
| `targetToggle.SetValue(initialToggleValue);` | Sets toggle value and notifies listeners. | `[LOG] SetValue applied. Toggle value: True` |
| `targetToggle.AddListener(OnToggleValueChanged);` | Adds a listener for toggle value change. | `[LOG] AddListener applied: Toggle change will trigger console log.` |
| `targetToggle.SetValue(!initialToggleValue, false);` | Sets value silently without triggering listeners. | `[LOG] SetValue(false, notify: false) applied. Toggle value changed without triggering listener.` |
| `targetSlider.SetRange(minSliderValue, maxSliderValue);` | Sets min and max values in one call. | `[LOG] SetRange applied. Min: 0, Max: 100` |
| `targetSlider.SetWholeNumbers(true);` | Forces slider to only use integers. | `[LOG] SetWholeNumbers applied. Slider now only returns integers.` |
| `targetSlider.SetValue(50f);` | Sets the slider value and notifies listeners. | `[LOG] SetValue applied. Slider value: 50` |
| `targetSlider.AddListener(OnSliderValueChanged);` | Adds a listener for slider value change. | `[LOG] AddListener applied: Slider change will trigger console log.` |

# Sprite Renderer Extensions – Usage Examples 
| Code Example | Description | Expected Console Output |
| :--- | :--- | :--- |
| `targetSpriteRenderer.ResetColor();` | Resets color to pure white with full alpha (1.0). | `1a. ResetColor applied. Current Alpha: 1` |
| `targetSpriteRenderer.SetAlpha(targetAlpha);` | Changes only the alpha value, keeps RGB the same. | `1b. SetAlpha(0.3) applied. Current Alpha: 0.3` |
| `targetSpriteRenderer.SetColor(newColor);` | Applies a new RGB color without touching the alpha. | `1c. SetColor(Red) applied. Current Color: RGBA(1,0,0,0.3)` |
| `targetSpriteRenderer.SetColorAndAlpha(Color.blue, 1f);` | Sets both color and alpha in a single call. | `1d. SetColorAndAlpha(Blue,1.0) applied. Current Color: RGBA(0,0,1,1)` |
| `targetSpriteRenderer.ToggleVisibility();` | Toggles alpha between 0 and 1. | `1e. ToggleVisibility applied. Is Visible?: False` |
| `targetSpriteRenderer.ToggleVisibility();` | Toggles visibility back. | `1f. ToggleVisibility applied again. Is Visible?: True` |
| `targetSpriteRenderer.SetSprite(newSprite);` | Updates SpriteRenderer’s sprite reference. | `2a. SetSprite applied: Sprite source updated.` |
| `targetSpriteRenderer.SetSortingOrder(newSortingOrder);` | Sets sorting order layer integer. | `2b. SetSortingOrder applied. Sorting Order: 100` |
| `targetSpriteRenderer.SetSortingLayer(newSortingLayerName);` | Assigns a sorting layer by name. | `2c. SetSortingLayer applied. Layer Name: 'UI'` |

# Material Extensions – Usage Examples 
| Code Example | Description | Expected Console Output |
| :--- | :--- | :--- |
| `_targetMaterial.SetMainColor(newMainColor);` | Sets the material’s main (albedo) color. | `1a. SetMainColor applied: Main color set to RGBA(0,1,0,1).` |
| `_targetMaterial.SetAlpha(targetAlpha);` | Updates only the alpha channel of the material’s main color. | `1b. SetAlpha applied: Main color alpha set to 0.5.` |
| `_targetMaterial.SetEmissionColor(emissionColor);` | Sets emission color (requires emission-enabled shader). | `1c. SetEmissionColor applied: Emission color set to RGBA(0,0,1,1).` |
| `_targetMaterial.SetColor("_SpecColor", Color.yellow);` | Generic color setter using a shader property name. | `1d. SetColor('_SpecColor', Yellow) applied.` |
| `_targetMaterial.SetOffset(newTextureOffset);` | Sets the main texture UV offset (scrolling effect). | `2a. SetOffset applied: UV offset set to (0.2, 0.5).` |
| `_targetMaterial.SetTiling(newTextureTiling);` | Sets UV tiling scale of the main texture. | `2b. SetTiling applied: UV scale set to (2, 2).` |
| `_targetMaterial.SetFloat(floatPropertyName, floatPropertyValue);` | Sets a float property on the shader if it exists. | `3a. SetFloat('_Glossiness', 0.8) applied.` |
| `_targetMaterial.Log("message");` | Logs material info using the built-in Log extension. | `3b. Log method called: You can chain .Log()...` |

# MeshRenderer Extensions – Usage Examples 
| Code Example | Description | Expected Console Output |
| :--- | :--- | :--- |
| `targetRenderer.IsRendererActive();` | Checks if the MeshRenderer component is enabled. | `1a. Initial State: Renderer Active? True/False` |
| `targetRenderer.ToggleRenderer(false);` | Disables the renderer, making the object invisible. | `1b. ToggleRenderer(false) applied. Renderer Active? False` |
| `targetRenderer.ToggleRenderer(true);` | Enables the renderer again. | `1c. ToggleRenderer(true) applied. Renderer Active? True` |
| `targetRenderer.SetShadowCastingMode(newShadowMode);` | Sets shadow casting mode (On, Off, TwoSided, OnlyShadows). | `1d. SetShadowCastingMode applied: Mode is now Off` |
| `targetRenderer.SetReceiveShadows(shouldReceiveShadows);` | Enables/disables receiving shadows. | `1e. SetReceiveShadows applied: Receives shadows? False` |
| `targetRenderer.ToggleCastShadows();` | Toggles between casting shadows or not. | `1f. ToggleCastShadows called. New Shadow Mode: On/Off` |
| `targetRenderer.SetReflectionProbeUsage(newReflectionProbeUsage);` | Sets how the renderer interacts with reflection probes. | `1g. SetReflectionProbeUsage applied: Usage is BlendProbes` |
| `targetRenderer.GetMaterial();` | Returns the renderer’s first material instance. | `2a. GetMaterial applied. Initial Material name: 'MaterialName'` |
| `targetRenderer.SetMaterial(newMaterialInstance);` | Replaces the primary material with the provided instance. | `2b. SetMaterial applied. Primary material changed to: 'NewMaterial'` |
| `targetRenderer.SetMaterials(new Material[] { initialMaterial });` | Replaces all assigned materials with a new list. | `2c. SetMaterials applied. Material array reset to the initial material.` |

# Camera Extensions – Usage Examples 
| Code Example | Description | Expected Console Output |
| :--- | :--- | :--- |
| `Vector3 worldPos = targetCamera.ScreenToWorld(simulatedScreenPoint, targetZDepth);` | Converts a 2D screen point to a 3D world point at a specified Z depth. | `1a. Screen Point (500, 500) converted to World Point (Z=10): (x, y, z)` |
| `Vector3 viewportPoint = targetCamera.WorldToViewport(worldPos);` | Converts a world position to normalized viewport coordinates (0–1). | `1b. World Point (x, y) converted to Viewport: (0.52, 0.48)` |
| `Bounds orthoBounds = targetCamera.GetOrthographicBounds();` | Calculates the world-space bounds of the camera’s orthographic view. | `1c. Orthographic Bounds Center: (cx, cy), Size: (w x h)` |
| `targetCamera.SetClearColor(newBackgroundColor);` | Sets the camera background/clear color. | `2a. SetClearColor applied: Background color set to RGBA(0,0,1,1)` |
| `targetCamera.SetOrthographicSize(newOrthoSize);` | Sets the orthographic size (2D zoom). | `2b. SetOrthographicSize applied: Size is now 8` |
| `bool isTargetVisible = targetCamera.IsVisible(targetRenderer);` | Checks if the provided Renderer is within the camera frustum (visible). | `3a. IsVisible applied: Is the target renderer visible to the camera? True/False` |

# Time Extensions – Usage Examples 
| Code Example | Description | Expected Console Output |
| :--- | :--- | :--- |
| `string mmss = simulatedDuration.ToMinutesAndSeconds();` | Formats seconds into **MM:SS**. | `1a. 3675.45s in MM:SS format: 61:15` |
| `string mmssms = simulatedDuration.ToMinutesAndSeconds(true);` | Formats seconds into **MM:SS.ms** including milliseconds. | `1b. 3675.45s in MM:SS.ms format: 61:15.450` |
| `string hhmmss = simulatedDuration.ToHoursMinutesAndSeconds();` | Formats seconds into **HH:MM:SS**. | `1c. 3675.45s in HH:MM:SS format: 01:01:15` |
| `(0.0000001f).IsZero();` | Checks if value is effectively zero using float-epsilon tolerance. | `1d. Is duration 0.0000001f considered zero? True` |
| `(1.0f).ToFrames();` | Converts seconds into frame count based on current fixedDeltaTime. | `1e. 1 second is equal to approximately 50 frames` |
| `TimeExtensions.TogglePause();` | Toggles `Time.timeScale` between 0 and 1. | `2a. TogglePause() called. Game is now: PAUSED/RESUMED` |
| `string formatted = _runningTime.ToMinutesAndSeconds(true);` | Continuously formats running time in Update loop. | `Current Time: 00:12.350` |
| `TimeExtensions.ResumeTime();` | Ensures timeScale becomes 1.0. | `Time started. Press 'P' to toggle pause/resume.` |

# MonoBehaviour Extensions – Usage Examples 
| Code Example | Description | Expected Console Output |
| :--- | :--- | :--- |
| `this.StartCoroutineSafe(ColorBlinkRoutine());` | Starts a coroutine safely, ensuring the reference is valid. | `1a. Color Blink Routine STARTED safely. Ref: Coroutine` |
| `this.StopCoroutineSafe(_colorBlinkRoutine);` | Stops a coroutine only if both MonoBehaviour and reference exist. | `1b. StopCoroutineSafe called after 5s delay. Routine should STOP.` |
| `this.RestartCoroutine(OneShotFlash(), ref _colorBlinkRoutine);` | Stops the previous routine and starts a new one in one call. | `1c. RestartCoroutine applied. Object flashing once now.` |
| `this.Delay(5f, () => StopBlinkRoutine());` | Executes an action safely after a delay (Invoke replacement). | `1b. StopCoroutineSafe called after 5s delay. Routine should STOP.` |
| `this.Delay(actionDelay, () => {...});` | Runs an action after a specified number of seconds. | `2a. Delay(2.5s) executed successfully.` |
| `this.DelayOneFrame(() => {...});` | Executes the action on the next frame. | `2b. DelayOneFrame executed.` |
| `this.DelayUntil(() => isConditionMet, () => {...});` | Waits until the condition is TRUE before firing the action. | `2c. DelayUntil executed! Condition was finally met.` |
| `StartCoroutine(ColorBlinkRoutine());` (internal) | Coroutine that alternates material colors repeatedly. | `Color switches between red/blue every blinkDuration` |
| `StartCoroutine(OneShotFlash());` | One-time color flash routine. | `One-shot flash complete.` |

# MathF Extensions – Usage Examples 
| Code Example | Description | Expected Console Output |
| :--- | :--- | :--- |
| `inputValue.Remap(0f, originalMax, 0f, targetMax);` | Maps a value from one range to another (e.g., HP → slider). | `1a. Remap: Value 50 (0 to 100) -> 0.50 (0 to 1).` |
| `inputValue.Normalize(0f, originalMax);` | Normalizes a value to 0–1 after clamping. | `1b. Normalize: Value 50 (0 to 100) -> 0.50 (0 to 1).` |
| `excessiveAngle.Wrap(0f, 360f);` | Wraps angles beyond 360 back into the valid range. | `2a. Wrap: 450 degrees wrapped (0-360) -> 90 degrees.` |
| `negativeAngle.Wrap(0f, 360f);` | Wraps negative angles into the correct positive range. | `2b. Wrap: -30 degrees wrapped (0-360) -> 330 degrees.` |
| `signCheckValue.GetSign();` | Returns +1 or -1 depending on float sign. | `2c. GetSign: Sign of -12.3 is -1.` |
| `rangeCheckValue.IsInRange(0, 10);` | Checks if an integer lies within a min–max range. | `3a. IsInRange(int): Is 5 in the range [0, 10]? True` |
| `inputValue.IsInRange(50f, 60f);` | Checks if a float lies within a min–max range. | `3b. IsInRange(float): Is 50 in the range [50, 60]? True` |
| `powerOfTwoValue.IsPowerOfTwo();` | Checks if a number is a power of two. | `3c. IsPowerOfTwo: Is 16 a power of two? True` |
| `20.NextPowerOfTwo();` | Calculates the next nearest power of two. | `3d. NextPowerOfTwo: The next power of two after 20 is 32.` |
| `1.00000001f.IsApproximately(1.0f);` | Float comparison using tolerance. | `3e. IsApproximately: Are 1.00000001f and 1.0f equal? True` |



## 1. Transform Extensions

### 1.1 Reset & Basic Setters

#### 🔹 ResetLocalPosition

| Code Example                        | Normal Code                                 | Description                                                                 |
| :---------------------------------- | :------------------------------------------ | :-------------------------------------------------------------------------- |
| `child.ResetLocalPosition();`       | `child.localPosition = Vector3.zero;`       | Resets the Transform’s **local position** to `(0,0,0)` without touching rotation or scale. |

#### 🔹 SetLocalScale

| Code Example                        | Normal Code                                                             | Description                                                           |
| :---------------------------------- | :---------------------------------------------------------------------- | :-------------------------------------------------------------------- |
| `child.SetLocalScale(y: 2f);`       | `var s = child.localScale; s.y = 2f; child.localScale = s;`             | Updates one or more **local scale axes** while preserving the others. |

#### 🔹 SetLocalRotationEuler

| Code Example                                 | Normal Code                                                                          | Description                                                               |
| :------------------------------------------- | :----------------------------------------------------------------------------------- | :------------------------------------------------------------------------ |
| `child.SetLocalRotationEuler(z: 90f);`       | `var e = child.localEulerAngles; e.z = 90f; child.localEulerAngles = e;`            | Sets specific **Euler rotation axes**, keeping the remaining axes intact. |

#### 🔹 ResetRotation

| Code Example                   | Normal Code                                        | Description                                  |
| :----------------------------- | :------------------------------------------------- | :------------------------------------------- |
| `child.ResetRotation();`       | `child.localRotation = Quaternion.identity;`       | Resets local rotation to identity `(0,0,0)`. |

#### 🔹 ResetScale

| Code Example                | Normal Code                             | Description                      |
| :-------------------------- | :-------------------------------------- | :------------------------------- |
| `child.ResetScale();`       | `child.localScale = Vector3.one;`       | Resets local scale to `(1,1,1)`. |

#### 🔹 SetPosition (world)

| Code Example                       | Normal Code                                                          | Description                                                 |
| :--------------------------------- | :------------------------------------------------------------------- | :---------------------------------------------------------- |
| `child.SetPosition(x: 10f);`       | `var p = child.position; p.x = 10f; child.position = p;`             | Sets specific **world position axes**, preserving the rest. |

#### 🔹 ResetLocal (all local properties)

| Code Example                | Normal Code                                                    | Description                                                   |
| :-------------------------- | :------------------------------------------------------------- | :------------------------------------------------------------ |
| `child.ResetLocal();`       | `localPosition = 0; localRotation = identity; localScale = 1;` | Resets local position, rotation, and scale in one call.       |

#### 🔹 ResetWorld

| Code Example                | Normal Code                                                | Description                         |
| :-------------------------- | :--------------------------------------------------------- | :---------------------------------- |
| `child.ResetWorld();`       | `position = Vector3.zero; rotation = Quaternion.identity;` | Resets **world** position and rotation. |

---

### 1.2 Hierarchy Helpers

#### 🔹 SetParentAndReset

| Code Example                                   | Normal Code                                                | Description                                                   |
| :--------------------------------------------- | :--------------------------------------------------------- | :------------------------------------------------------------ |
| `child.SetParentAndReset(targetParent);`       | `SetParent(targetParent); Reset local transform manually.` | Assigns new parent and resets local position/rotation/scale. |

#### 🔹 GetChildren

| Code Example                             | Normal Code                                        | Description                                  |
| :--------------------------------------- | :------------------------------------------------- | :------------------------------------------- |
| `var list = parent.GetChildren();`       | `Manual loop adding each child to List<Transform>` | Returns all direct children as a **List**.   |

#### 🔹 GetSiblingIndexChecked

| Code Example                                        | Normal Code                                           | Description                                                   |
| :-------------------------------------------------- | :---------------------------------------------------- | :------------------------------------------------------------ |
| `int index = child.GetSiblingIndexChecked();`       | `child.parent != null ? GetSiblingIndex() : -1`       | Safe sibling index; returns `-1` if the Transform has no parent. |

---

## 2. GameObject Extensions

### 2.1 Component Management

#### 🔹 GetOrAddComponent\<T\>

| Code Example                                                   | Normal Code                                                                                                                         | Description                                              |
| :------------------------------------------------------------- | :---------------------------------------------------------------------------------------------------------------------------------- | :------------------------------------------------------- |
| `comp = go.GetOrAddComponent<TestComponent>();`                | `var comp = go.GetComponent<TestComponent>(); if (comp == null) comp = go.AddComponent<TestComponent>();`                           | Gets existing component or adds it if missing.           |

#### 🔹 HasComponent\<T\>

| Code Example                                               | Normal Code                                        | Description                                              |
| :--------------------------------------------------------- | :------------------------------------------------- | :------------------------------------------------------- |
| `bool hasComp = go.HasComponent<TestComponent>();`         | `go.GetComponent<TestComponent>() != null`         | Quickly checks if a GameObject has a specific component. |

#### 🔹 RemoveComponent\<T\>

| Code Example                                   | Normal Code                                                                                                | Description                                                         |
| :--------------------------------------------- | :--------------------------------------------------------------------------------------------------------- | :------------------------------------------------------------------ |
| `go.RemoveComponent<TestComponent>();`         | `var comp = go.GetComponent<TestComponent>(); if (comp != null) DestroyImmediate(comp);`                  | Safely removes a component (Editor/Runtime safe).                  |

---

### 2.2 Activation & State

#### 🔹 ToggleActive

| Code Example                 | Normal Code                                     | Description                            |
| :--------------------------- | :---------------------------------------------- | :------------------------------------- |
| `go.ToggleActive();`         | `go.SetActive(!go.activeSelf);`                 | Inverts the GameObject's active state. |

#### 🔹 IsActiveAndEnabled

| Code Example                       | Normal Code                    | Description                                            |
| :--------------------------------- | :----------------------------- | :----------------------------------------------------- |
| `go.IsActiveAndEnabled();`         | `go.activeInHierarchy`         | Checks if object is fully active in the hierarchy.     |

#### 🔹 SetChildrenActive

| Code Example                          | Normal Code                                                                   | Description                                                      |
| :------------------------------------ | :---------------------------------------------------------------------------- | :--------------------------------------------------------------- |
| `parent.SetChildrenActive(true);`     | `foreach (Transform t in parent.transform) t.gameObject.SetActive(true);`     | Sets active state for all immediate children.                    |

#### 🔹 SetChildActiveAt

| Code Example                             | Normal Code                                                                                              | Description                                               |
| :--------------------------------------- | :------------------------------------------------------------------------------------------------------- | :-------------------------------------------------------- |
| `parent.SetChildActiveAt(0, false);`     | `if (0 < parent.transform.childCount) parent.transform.GetChild(0).gameObject.SetActive(false);`        | Safely sets active state for a child at a specific index. |

---

### 2.3 Hierarchy, Tag & Layer

#### 🔹 InstantiateAsChild

| Code Example                                                              | Normal Code                                                                                           | Description                                        |
| :------------------------------------------------------------------------ | :---------------------------------------------------------------------------------------------------- | :------------------------------------------------- |
| `var clone = go.InstantiateAsChild(parentTransform);`                     | `var clone = Instantiate(go); clone.transform.SetParent(parentTransform, false);`                     | Clones object and parents it in a single call.     |

#### 🔹 Rename

| Code Example                | Normal Code                | Description                            |
| :-------------------------- | :------------------------- | :------------------------------------- |
| `clone.Rename("Enemy_01");` | `clone.name = "Enemy_01";` | Changes the GameObject's name.        |

#### 🔹 SetLayerRecursive

| Code Example                       | Normal Code                                                                                        | Description                                             |
| :--------------------------------- | :------------------------------------------------------------------------------------------------- | :------------------------------------------------------ |
| `go.SetLayerRecursive(1);`         | `foreach (Transform t in go.GetComponentsInChildren<Transform>()) t.gameObject.layer = 1;`        | Sets layer for object and all children recursively.     |

#### 🔹 SetTagRecursive

| Code Example                             | Normal Code                                                                                              | Description                                           |
| :--------------------------------------- | :------------------------------------------------------------------------------------------------------- | :---------------------------------------------------- |
| `go.SetTagRecursive("Respawn");`         | `foreach (Transform t in go.GetComponentsInChildren<Transform>()) t.gameObject.tag = "Respawn";`         | Sets tag for object and all children recursively.     |

#### 🔹 DestroyChildAt

| Code Example                    | Normal Code                                                                                      | Description                                                     |
| :------------------------------ | :----------------------------------------------------------------------------------------------- | :-------------------------------------------------------------- |
| `go.DestroyChildAt(0);`         | `if (0 < go.transform.childCount) Destroy(go.transform.GetChild(0).gameObject);`               | Safely destroys a child by index with bounds checking.          |

---

## 3. Vector Extensions

### 3.1 Axis Manipulation

#### 🔹 With()

| Code Example                                            | Normal Code                                                           | Description                                            |
| :------------------------------------------------------ | :-------------------------------------------------------------------- | :----------------------------------------------------- |
| `var v2 = pos.With(y: 20f);`                            | `var v2 = pos; v2.y = 20f;`                                           | Sets Y while preserving X and Z.                      |

#### 🔹 SetX / SetY / SetZ

| Code Example                         | Normal Code                                    | Description                                            |
| :----------------------------------- | :--------------------------------------------- | :----------------------------------------------------- |
| `var v2 = pos.SetX(1f);`             | `var v2 = pos; v2.x = 1f;`                     | Sets X while preserving Y and Z.                      |

---

### 3.2 Approximation & Helpers

#### 🔹 IsApproximately

| Code Example                                                              | Normal Code                                                                                                                  | Description                                                                                                 |
| :------------------------------------------------------------------------ | :--------------------------------------------------------------------------------------------------------------------------- | :---------------------------------------------------------------------------------------------------------- |
| `pos.IsApproximately(target, 0.001f);`                                    | `var d = (pos - target).sqrMagnitude; bool close = d < (0.001f * 0.001f);`                                                   | Checks if two vectors are approximately equal (float-safe).            |

#### 🔹 DirectionTo

| Code Example                                           | Normal Code                         | Description                                                  |
| :----------------------------------------------------- | :---------------------------------- | :----------------------------------------------------------- |
| `var dir = from.DirectionTo(to);`                      | `(to - from).normalized`            | Normalized direction from source to target.                 |

#### 🔹 IsZero

| Code Example                               | Normal Code                                     | Description                                                              |
| :----------------------------------------- | :---------------------------------------------- | :----------------------------------------------------------------------- |
| `v.IsZero();`                              | `v.sqrMagnitude < (0.0001f * 0.0001f)`          | Checks if vector is effectively zero.                                    |

#### 🔹 Abs

| Code Example                                     | Normal Code                                                   | Description                                               |
| :----------------------------------------------- | :------------------------------------------------------------ | :-------------------------------------------------------- |
| `var absV = v.Abs();`                            | `new Vector3(Mathf.Abs(v.x), Mathf.Abs(v.y), Mathf.Abs(v.z))` | Returns vector with absolute values.                     |

---

### 3.3 Vector2 ⇄ Vector3

#### 🔹 ToVector3

| Code Example                   | Normal Code                   | Description                                    |
| :----------------------------- | :---------------------------- | :--------------------------------------------- |
| `Vector3 v3 = v2.ToVector3();` | `new Vector3(v2.x, v2.y, 0f)` | Converts Vector2 to Vector3 (z = 0).           |

---

## 4. Number Extensions (Float & Int)

### 4.1 Float – Checks & Comparison

#### 🔹 IsApproximately

| Code Example                                                           | Normal Code                                          | Description                                                             |
| :--------------------------------------------------------------------- | :--------------------------------------------------- | :---------------------------------------------------------------------- |
| `f.IsApproximately(other, 0.001f);`                                    | `Mathf.Abs(f - other) <= 0.001f`                     | Float comparison with tolerance.                                       |

#### 🔹 IsBetween (float)

| Code Example                                | Normal Code            | Description                                                |
| :------------------------------------------ | :--------------------- | :--------------------------------------------------------- |
| `f.IsBetween(10f, 20f);`                    | `f >= 10f && f <= 20f` | Checks if float lies in a given inclusive range.          |

#### 🔹 IsNegative / IsPositive

| Code Example                     | Normal Code | Description                         |
| :------------------------------- | :---------- | :---------------------------------- |
| `f.IsNegative();`                | `f < 0`     | Checks if value is negative.        |

#### 🔹 ToNormalizedPercentage

| Code Example                                           | Normal Code  | Description                                                       |
| :----------------------------------------------------- | :----------- | :---------------------------------------------------------------- |
| `normalized = value.ToNormalizedPercentage(max);`      | `value / max` | Turns a value into a 0–1 range of a total.                       |

---

### 4.2 Float – Formatting & Rounding

#### 🔹 ToTimeFormat

| Code Example                                           | Normal Code                                                                                                                                               | Description                                               |
| :----------------------------------------------------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------- | :-------------------------------------------------------- |
| `time.ToTimeFormat();`                                 | `TimeSpan t = TimeSpan.FromSeconds(time); string s = $"{t.Minutes}:{t.Seconds:00}.{t.Milliseconds / 10:00}";`                                             | Formats seconds as `"m:ss.ff"`.                          |

#### 🔹 RoundTo

| Code Example                           | Normal Code                                       | Description                                               |
| :------------------------------------- | :------------------------------------------------ | :-------------------------------------------------------- |
| `value.RoundTo(2);`                    | `(float)Math.Round(value, 2);`                    | Rounds float to given decimals.                          |

#### 🔹 ToDegrees

| Code Example                        | Normal Code                | Description                         |
| :---------------------------------- | :------------------------- | :---------------------------------- |
| `Mathf.PI.ToDegrees();`             | `Mathf.PI * Mathf.Rad2Deg` | Radians → degrees.                 |

---

### 4.3 Int – Checks

#### 🔹 IsEven / IsOdd

| Code Example        | Normal Code  | Description                   |
| :------------------ | :----------- | :---------------------------- |
| `i.IsEven();`       | `i % 2 == 0` | True if even.                 |
| `i.IsOdd();`        | `i % 2 != 0` | True if odd.                  |

#### 🔹 IsBetween (int)

| Code Example                      | Normal Code           | Description                                                   |
| :-------------------------------- | :-------------------- | :------------------------------------------------------------ |
| `i.IsBetween(-10, -1);`           | `i >= -10 && i <= -1` | Inclusive integer range check.                                |

#### 🔹 IsPositive

| Code Example                | Normal Code | Description                                             |
| :-------------------------- | :---------- | :------------------------------------------------------ |
| `i.IsPositive();`           | `i > 0`     | Checks if integer is strictly greater than zero.       |

---

## 5. Collection Extensions (List & Array)

### 5.1 Safe Access

#### 🔹 IsNullOrEmpty

| Code Example            | Normal Code                                | Description                                  |
| :---------------------- | :----------------------------------------- | :------------------------------------------- |
| `names.IsNullOrEmpty()` | `names == null || names.Count == 0`        | Checks if collection is null or empty.       |

#### 🔹 GetSafe\<T\>

| Code Example                              | Normal Code                                                                        | Description                                                                  |
| :---------------------------------------- | :--------------------------------------------------------------------------------- | :--------------------------------------------------------------------------- |
| `string s = names.GetSafe(99);`           | `if (index >= 0 && index < names.Count) return names[index]; else return default;` | Safely gets element or returns default.                                     |

#### 🔹 TryGet\<T\>

| Code Example                                         | Normal Code                                                                              | Description                                                                    |
| :--------------------------------------------------- | :--------------------------------------------------------------------------------------- | :----------------------------------------------------------------------------- |
| `if (names.TryGet(1, out var found)) { ... }`        | `if (1 >= 0 && 1 < names.Count) { found = names[1]; } else { found = default; }`        | Safe getter returning bool and output value.                                  |

---

### 5.2 Randomness & Ordering

#### 🔹 RandomElement (List / Array)

| Code Example                                 | Normal Code                                                                   | Description                             |
| :------------------------------------------- | :---------------------------------------------------------------------------- | :-------------------------------------- |
| `var randomName = names.RandomElement();`    | `var i = Random.Range(0, names.Count); var randomName = names[i];`           | Picks a random element.                 |

#### 🔹 Shuffle

| Code Example       | Normal Code                                           | Description                           |
| :----------------- | :---------------------------------------------------- | :------------------------------------ |
| `names.Shuffle();` | `// Implement Fisher-Yates manually`                  | Randomly reorders list elements.      |

#### 🔹 RemoveRandom

| Code Example                             | Normal Code                                                                                       | Description                              |
| :--------------------------------------- | :------------------------------------------------------------------------------------------------ | :--------------------------------------- |
| `string removed = names.RemoveRandom();` | `int index = Random.Range(0, names.Count); removed = names[index]; names.RemoveAt(index);`        | Removes and returns a random element.    |

---

### 5.3 Bulk Operations

#### 🔹 AddRangeUnique

| Code Example                                         | Normal Code                                                                      | Description                                                                         |
| :--------------------------------------------------- | :------------------------------------------------------------------------------- | :---------------------------------------------------------------------------------- |
| `int count = names.AddRangeUnique(itemsToAdd);`      | `foreach(var item in itemsToAdd) if(!names.Contains(item)) names.Add(item);`    | Adds only non-existing items, returns how many were added.                         |

#### 🔹 RemoveAll

| Code Example                                            | Normal Code                                                  | Description                                                    |
| :------------------------------------------------------ | :----------------------------------------------------------- | :------------------------------------------------------------- |
| `names.RemoveAll(itemsToRemove);`                       | `foreach(var item in itemsToRemove) { names.Remove(item); }` | Removes all items that are present in another collection.      |

---

## 6. String Extensions

### 6.1 Validation & Checking

#### 🔹 IsNullOrEmpty / IsNullOrWhiteSpace

| Code Example                            | Normal Code                                   | Description                                                          |
| :-------------------------------------- | :-------------------------------------------- | :------------------------------------------------------------------- |
| `s.IsNullOrEmpty();`                    | `string.IsNullOrEmpty(s)`                     | Checks null or empty.                                                |
| `s.IsNullOrWhiteSpace();`              | `string.IsNullOrWhiteSpace(s)`                | Checks null/empty or only whitespace.                                |

#### 🔹 ContainsAny

| Code Example                                   | Normal Code                                                                         | Description                                                    |
| :--------------------------------------------- | :---------------------------------------------------------------------------------- | :------------------------------------------------------------- |
| `s.ContainsAny(searchValues);`                 | `foreach (var v in searchValues) if (s.Contains(v)) return true;`                  | True if any of the substrings is contained.                    |

#### 🔹 IsNumeric

| Code Example                | Normal Code                            | Description                                     |
| :-------------------------- | :------------------------------------- | :---------------------------------------------- |
| `s.IsNumeric();`            | `float.TryParse(s, out _)`             | True if string can be parsed as number.         |

---

### 6.2 Case, Length & Cleaning

#### 🔹 GetLength

| Code Example                 | Normal Code             | Description                       |
| :--------------------------- | :---------------------- | :-------------------------------- |
| `s.GetLength();`             | `s.Length`              | Returns string length.            |

#### 🔹 RemoveChar

| Code Example                                 | Normal Code                                                        | Description                                                     |
| :------------------------------------------- | :----------------------------------------------------------------- | :-------------------------------------------------------------- |
| `s.RemoveChar('a');`                         | `s.Replace("a", string.Empty);`                                    | Removes all instances of a specific char.                      |

#### 🔹 Cut

| Code Example                 | Normal Code                                                             | Description                                                 |
| :--------------------------- | :---------------------------------------------------------------------- | :---------------------------------------------------------- |
| `s.Cut(maxLength);`          | `if (s.Length > maxLength) s = s.Substring(0, maxLength) + "...";`      | Truncates and adds "…" if too long.                        |

#### 🔹 RemoveWhitespace

| Code Example                     | Normal Code                                                | Description                                                 |
| :------------------------------- | :--------------------------------------------------------- | :---------------------------------------------------------- |
| `"a b\tc\nd".RemoveWhitespace()` | `Replace(" ", "").Replace("\t", "").Replace("\n", "")`     | Removes spaces, tabs, and newlines.                        |

---

## 7. UI Text & TMP Extensions

> Applies to **Text** or **TextMeshProUGUI** depending on your implementation.

Examples:

- `text.SetTextAndColor("Status: OK", Color.green);`
- `text.SetAlpha(0.5f);`
- `text.SetTextSafe("Hello");`
- `text.IsTextSet();`
- `text.ClearText();`
- `text.ToggleVisibility();` (alpha 0 ↔ 1)
- `"Score".ToBold().Colorize(Color.red).Size(150);`
- `StartCoroutine(text.FadeToAlpha(1f, 1.5f));`
- `text.SetTextFormat("Score: {0}", score);`

These helpers make it easy to apply formatting, fades and rich tags without repeating boilerplate.

---

## 8. Image / Button / Toggle / Slider Extensions

### 8.1 Image

- `image.SetColor(color);`
- `image.SetAlpha(0.5f);`
- `image.SetSprite(newSprite);`
- `image.ToggleVisibility();` (alpha 0 ↔ 1)

### 8.2 Button

- `button.SetInteractable(true);`
- `button.ToggleInteractable();`
- `button.AddListener(() => OnClicked());`

### 8.3 Toggle

- `toggle.SetValue(true);`
- `toggle.AddListener(OnToggleChanged);`
- `toggle.SetValue(false, notify: false);` (no events)

### 8.4 Slider

- `slider.SetRange(0, 100);`
- `slider.SetWholeNumbers(true);`
- `slider.SetValue(50f);`
- `slider.AddListener(OnSliderChanged);`

---

## 9. Renderer & Material Extensions

### 9.1 SpriteRenderer

- `sprite.ResetColor();`
- `sprite.SetAlpha(0.3f);`
- `sprite.SetColor(Color.red);`
- `sprite.SetColorAndAlpha(Color.blue, 1f);`
- `sprite.ToggleVisibility();`
- `sprite.SetSprite(newSprite);`
- `sprite.SetSortingOrder(100);`
- `sprite.SetSortingLayer("UI");`

### 9.2 Material

- `_mat.SetMainColor(Color.green);`
- `_mat.SetAlpha(0.5f);`
- `_mat.SetEmissionColor(Color.blue);`
- `_mat.SetColor("_SpecColor", Color.yellow);`
- `_mat.SetOffset(new Vector2(0.2f, 0.5f));`
- `_mat.SetTiling(new Vector2(2, 2));`
- `_mat.SetFloat("_Glossiness", 0.8f);`

### 9.3 MeshRenderer

- `renderer.IsRendererActive();`
- `renderer.ToggleRenderer(false);`
- `renderer.SetShadowCastingMode(ShadowCastingMode.Off);`
- `renderer.SetReceiveShadows(false);`
- `renderer.ToggleCastShadows();`
- `renderer.SetReflectionProbeUsage(ReflectionProbeUsage.BlendProbes);`
- `var mat = renderer.GetMaterial();`
- `renderer.SetMaterial(newMat);`
- `renderer.SetMaterials(new Material[] { mat });`

---

## 10. Camera Extensions

- `camera.ScreenToWorld(screenPoint, zDepth);`
- `camera.WorldToViewport(worldPos);`
- `Bounds b = camera.GetOrthographicBounds();`
- `camera.SetClearColor(Color.blue);`
- `camera.SetOrthographicSize(8f);`
- `bool visible = camera.IsVisible(renderer);`

---

## 11. Time Extensions

- `timeInSeconds.ToMinutesAndSeconds();` → `"MM:SS"`
- `timeInSeconds.ToMinutesAndSeconds(true);` → `"MM:SS.ms"`
- `timeInSeconds.ToHoursMinutesAndSeconds();` → `"HH:MM:SS"`
- `0.0000001f.IsZero();` → float epsilon check
- `1.0f.ToFrames();` → seconds → frames (based on `fixedDeltaTime`)
- `TimeExtensions.TogglePause();` → toggles `Time.timeScale` 0 ↔ 1
- `TimeExtensions.ResumeTime();` → sets `timeScale = 1`

---

## 12. MonoBehaviour Extensions

- `this.StartCoroutineSafe(MyRoutine());`
- `this.StopCoroutineSafe(_routineRef);`
- `this.RestartCoroutine(MyRoutine(), ref _routineRef);`
- `this.Delay(2.5f, () => DoSomething());`
- `this.DelayOneFrame(() => DoSomething());`
- `this.DelayUntil(() => ready, () => DoSomething());`

These are safer replacements for `StartCoroutine`, `StopCoroutine`, and `Invoke`, avoiding null reference issues.

---

## 13. Enum & Flags Extensions (Examples)

- `currentDifficulty.GetDescription();`
- `currentDifficulty.GetName();`
- `Difficulty next = currentDifficulty.Next();`
- `Difficulty prev = currentDifficulty.Previous();`
- `var values = EnumExtensions.GetValues<Difficulty>();`
- `abilities = abilities.SetFlag(PlayerAbilities.Shield, true);`

Makes enum cycling, description reading and flag handling much easier.

---

## 14. MathF Extensions

- `value.Remap(0f, originalMax, 0f, targetMax);`
- `value.Normalize(0f, originalMax);`
- `angle.Wrap(0f, 360f);`
- `value.GetSign();`
- `i.IsInRange(0, 10);`
- `f.IsInRange(50f, 60f);`
- `n.IsPowerOfTwo();`
- `n.NextPowerOfTwo();`
- `f.IsApproximately(1.0f);`

---

## Tips for Usage

- Add your extension namespaces in a **central using file** or top of each script.
- Prefer these helpers to reduce **boilerplate** and keep code **clean & readable**.
- If something is missing, you can easily add a new extension following the same patterns.

Happy coding 🚀
