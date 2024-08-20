## The general idea of Jetpack Compose
- Describe what, not how (declarative vs imperative)
- UI elements are functions
- State controls UI
- Events determine state
## Composables, Recomposition
- Encourage reusable UI
- Shouldn't modify global properties and variable => behave the same way every time being called => idempotent
Example:
```kotlin
@Composable
fun SingleChoiceQuestion(answers: List<Answer>)
{
	Column {
	if (answers.isEmpty()){
		Text("See? You can just if else, no setting Visibility bs!!!")
	} else {
			answers.forEach{ answer -> 
				SurveyAnswer(answer = answer) // this will create an UI element for each answer of answers
			}
		}
	}
}
```

- Parameters here completely controls the UI (State into UI).
- If answers changed, new UI will be created from this function => recomposition (also apply when internal state changes).
- `remember` is important, to remember the state whenever it gets recomposed, but it wouldn't survive config changes (Screen rotation, language change,...) -> `rememberSaveable`
Example:
```kotlin
@Composable
fun SingleChoiceQuestion(answers: List<Answer>)
{
	// var selectedAnswer: Answer? = null // This doesn't work
	var selectedAnswer: Answer? by rememberSaveable { mutableStateOf(null) }// INotifyPropertyChanged flashback
	answers.forEach{ answer -> 
		SurveyAnswer(
		answer = answer,
		isSelected = (selectedAnswer == answer),//(selectedAnswer.value == answer)
		onAnswerSelected = { answer -> selectedAnswer = answer} //lambda func to set the selected answer to a new one
		) 

	}
}
```
![[Pasted image 20240820233703.png]]
## Behaviours
- Composable functions can execute in any order. It can recognize some higher priority UI elements and draw them first.
- Composable functions can run in parallel.
- Recomposition skips as much as possible. Only update where needed
- Recompostion is optimistic, it expects to be faster than the rate params being changed. But if that happens, recomposition will ditch it and restart.
- Composable functions might run frequently.