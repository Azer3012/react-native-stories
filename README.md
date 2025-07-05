# custom-react-native-stories

<p align="center">

</p>

## Install

#### 1. Step

```javascript
npm install custom-react-native-stories --save
```

or

```javascript
yarn add custom-react-native-stories
```

#### 2. Step

```javascript
cd ios && pod install
```

## Import

```javascript
import InstaStory from 'custom-react-native-stories';
```

## Props

| Name                       | Description                                         | Type          | Default Value |
| :------------------------- | :-------------------------------------------------- | :------------ | :-----------: |
| data                       | Array of IUserStory. You can check from interfaces. | object        |               |
| unPressedBorderColor       | Unpressed border color of profile circle            | color         |      red      |
| pressedBorderColor         | Pressed border color of profile circle              | color         |     grey      |
| unPressedAvatarTextColor   | Unpressed avatar text color                         | color         |      red      |
| pressedAvatarTextColor     | Pressed avatar text color                           | color         |     grey      |
| onStorySeen                | Called each time story is seen                      | function      |     null      |
| onClose                    | Todo when close                                     | function      |     null      |
| onStart                    | Todo when start                                     | function      |     null      |
| duration                   | Per story duration seconds                          | number        |      10       |
| swipeText                  | Text of swipe component                             | string        |   Swipe Up    |
| renderSwipeUpComponent     | Render a custom swipe up component                  | function      |               |
| renderCloseComponent       | Render a custom close button                        | function      |               |
| renderTextComponent        | Render custom avatar text component                 | function      |               |
| avatarSize                 | Size of avatar circle                               | number        |      60       |
| showAvatarText             | For show or hide avatar text.                       | bool          |     true      |
| avatarTextStyle            | For avatar text style                               | TextStyle     |               |
| avatarImageStyle           | For avatar image style                              | ImageStyle    |               |
| avatarWrapperStyle         | For individual avatar wrapper style                 | ViewStyle     |               |
| avatarFlatListProps        | Horizontal avatar flat list props                   | FlatListProps |               |
| loadedAnimationBarStyle    | For loaded animation bar style                      | ViewStyle     |               |
| unloadedAnimationBarStyle  | For unloaded animation bar style                    | ViewStyle     |               |
| animationBarContainerStyle | For animation bar container style                   | ViewStyle     |               |
| storyUserContainerStyle    | For story item user container style                 | ViewStyle     |               |
| storyImageStyle            | For story image style                               | ImageStyle    |               |
| storyAvatarImageStyle      | For story avatar image style                        | ImageStyle    |               |
| storyContainerStyle        | For story container style                           | ViewStyle     |               |

## Usage

### Basic

```javascript
import InstaStory from 'custom-react-native-stories';

const data = [
  {
    user_id: 1,
    user_image: 'https://..jpg',
    user_name: 'Azer Abishov',
    stories: [
      {
        story_id: 1,
        story_image: 'https://...jpg',
        swipeText: 'Custom swipe text for this story',
        onPress: () => console.log('story 1 swiped'),
      },
      {
        story_id: 2,
        story_image: 'https://...jpg',
      },
    ],
  },
  {
    user_id: 2,
    user_image: 'https://...jpg',
    user_name: 'Nurmehemmed Nebiyev',
    stories: [
      {
        story_id: 1,
        story_image: 'https://...jpg',
        swipeText: 'Custom swipe text for this story',
        onPress: () => console.log('story 1 swiped'),
      },
      {
        story_id: 2,
        story_image: 'https://...jpg',
        swipeText: 'Custom swipe text for this story',
        onPress: () => console.log('story 2 swiped'),
      },
    ],
  },
];

<InstaStory data={data} duration={10} />;
```

### Custom components

The render component functions are all passed `item` as a prop which is the current [IUserStoryItem](./src/interfaces/index.ts#L15) being displayed.

`renderSwipeUpComponent` and `renderCloseComponent` are both passed the `onPress` prop which is a function that closes the current story item modal and calls the `IUserStoryItem.onPress` function. `onPress` is passed so you could add other buttons. This is useful when adding a button which has it's own `onPress` prop, eg. a share button, next to the close button.

`renderTextComponent` is passed the `profileName` of the current story's user.

```javascript
const data = [...sameDataAsBasicExampleAbove];

const [seenStories, setSeenStories] = useState(new Set());

const updateSeenStories = ({ story: { story_id } }) => {
  setSeenStories((prevSet) => {
    prevSet.add(storyId);
    return prevSet;
  });
};

const handleSeenStories = async (item) => {
  console.log(item);
  const storyIds = [];
  seenStories.forEach((storyId) => {
    if (storyId) storyIds.push(storyId);
  });
  if (storyIds.length > 0) {
    await fetch('myApi', {
      method: 'POST',
      body: JSON.stringify({ storyIds }),
    });
    seenStories.clear();
  }
};

<InstaStory
  data={data}
  duration={10}
  onStart={(item) => console.log(item)}
  onClose={handleSeenStories}
  onStorySeen={updateSeenStories}
  renderCloseComponent={({ item, onPress }) => (
    <View style={{ flexDirection: 'row' }}>
      <Button onPress={shareStory}>Share</Button>
      <Button onPress={onPress}>X</Button>
    </View>
  )}
  renderTextComponent={({ item, profileName }) => (
    <View>
      <Text>{profileName}</Text>
      <Text>{item.customProps?.yourCustomProp}</Text>
    </View>
  )}
  style={{ marginTop: 30 }}
/>;
```
