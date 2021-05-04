
# Chat

A full-stack (Ruby on Rails + React/Redux) messaging app. It includes live messaging between users with group messaging and direct messaging, implemented with ActionCable (a WebSocket framework for Rails).

This project was completed in two weeks. The process begin with the design of the following MVPs:
- User Authentication (backend and frontend)
- Live Messaging
- Direct Chats
- Group Chats
- User + Conversation Search
- User profile images

# Technologies
- User authentication with BCrypt
- Users communicate with live updating (ActionCable)
- User profile image upload and updating using Amazon Web Services (AWS)
- Communication between frontend and backend with jQuery ajax
- Dynamic frontend (React + Redux)



## ActionCable live messaging
```javascript
class MessageFeed extends React.Component {
  createSocket(currentChatId) {
    let cable;
    if (process.env.NODE_ENV !== 'production') {
      cable = Cable.createConsumer('http://localhost:3000/cable');
    } else {
      cable = Cable.createConsumer('wss://chat.herokuapp.com/cable');
    }
    this.chats = cable.subscriptions.create({
      channel: "MessagesChannel",
      chatId: currentChatId
    }, {
      connected: () => {
      },
      disconnected: () => {
      },
      received: (data) => {
        this.props.receiveMessage(data);
      }
    });
  }
```

### User search
```javascript
class Api::UsersController < ApplicationController

  def search
    query = params['query'].downcase
    userPriorityHash = Hash.new {|h, k| h[k] = []}
    @users = []
    matched_users = User.all.each do |user|
      next if user.id == current_user.id
      priority = matched_name(user.first_name, user.last_name, query)
      userPriorityHash[priority] << user unless priority.nil?
    end
    (1..3).each do |n|
      userPriorityHash[n].each do |user|
        @users << user
      end
    end
    render :search
  end
```


