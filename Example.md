# CreatePostScreen

`CreatePostScreen` is a React component responsible for rendering a screen that allows users to create and submit a new post. The screen includes a post editor, media attachment functionality, and an optional location tagging feature.

## Imports

- `React`: React library with hooks for managing state and side effects.
- `BackHandler`, `ActivityIndicator`, `Platform`, `Button`: React Native components used for handling back button events, displaying activity indicators, and rendering buttons.
- `useTheme`, `useTranslations`, `Alert`: Custom hooks and components from the core `dopebase` module for handling theme, translations, and alerts.
- `CreatePost`: A custom component that renders the post creation form.
- `usePostMutations`: A custom hook for handling post-related mutations.
- `useSocialGraphFriends`: A custom hook for fetching the user's friends list.
- `useCurrentUser`: A custom hook for accessing the current user's data.

## State Variables

- `post`: A state variable that holds the current post content.
- `postMedia`: An array of local photo URLs attached to the post.
- `location`: The location associated with the post.
- `isPosting`: A boolean indicating if a post is currently being submitted.

## Methods

- `onBackButtonPressAndroid`: A method that handles the back button press on Android devices.
- `onPostDidChange`: A callback function that updates the post state when the post content changes.
- `onSetMedia`: A callback function that updates the `postMedia` state with the selected media files.
- `onLocationDidChange`: A callback function that updates the `location` state when the location changes.
- `onPost`: A method that submits the post and attached media, handling validation and error messages.
- `blurInput`: A method to blur the text input.

## Usage

This component should be used in a navigation stack, as it depends on `navigation` prop for navigation events and header customization. It should be rendered as a screen in your navigation structure.

```jsx
import { createStackNavigator } from '@react-navigation/stack';
import CreatePostScreen from './CreatePostScreen';

const Stack = createStackNavigator();

function App() {
  return (
    <NavigationContainer>
      <Stack.Navigator>
        {/* Other screens */}
        <Stack.Screen name="CreatePost" component={CreatePostScreen} />
      </Stack.Navigator>
    </NavigationContainer>
  );
}

export default App;
