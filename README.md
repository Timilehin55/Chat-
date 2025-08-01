# Chat
import { useState, useRef, useEffect } from 'react';

export default function App() {
  const [messages, setMessages] = useState([
    { id: 1, sender: 'Alice', text: 'Hey everyone! How was your weekend?', time: '10:00 AM' },
    { id: 2, sender: 'Bob', text: 'It was awesome! Went hiking in the mountains.', time: '10:02 AM' },
    { id: 3, sender: 'Charlie', text: 'I binge-watched a new series. So good!', time: '10:03 AM' },
    { id: 4, sender: 'Alice', text: 'Nice! We should plan something together soon.', time: '10:05 AM' },
    { id: 5, sender: 'Bob', text: 'Absolutely! What about this weekend?', time: '10:06 AM' },
  ]);
  
  const [newMessage, setNewMessage] = useState('');
  const [currentChat, setCurrentChat] = useState('friends');
  const messagesEndRef = useRef(null);
  const inputRef = useRef(null);

  const scrollToBottom = () => {
    messagesEndRef.current?.scrollIntoView({ behavior: 'smooth' });
  };

  useEffect(() => {
    scrollToBottom();
  }, [messages]);

  const handleSendMessage = (e) => {
    e.preventDefault();
    if (!newMessage.trim()) return;

    const message = {
      id: messages.length + 1,
      sender: 'You',
      text: newMessage,
      time: new Date().toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' }),
    };

    setMessages([...messages, message]);
    setNewMessage('');
    
    // Focus input after sending message
    setTimeout(() => {
      inputRef.current?.focus();
    }, 0);
  };

  const renderChat = () => {
    return (
      <div className="flex flex-col h-full">
        {/* Chat header */}
        <div className="bg-white border-b border-gray-200 px-4 py-3 shadow-sm flex items-center justify-between sticky top-0 z-10">
          <div className="flex items-center">
            <button className="mr-3 md:hidden" onClick={() => setCurrentChat('')}> 
              <svg xmlns="http://www.w3.org/2000/svg" className="h-5 w-5 text-gray-600" viewBox="0 0 20 20" fill="currentColor">
                <path fillRule="evenodd" d="M9.707 16.707a1 1 0 01-1.414 0l-6-6a1 1 1 0 010-1.414l6-6a1 1 1 0 011.414 1.414L5.414 9H17a1 1 0 110 2H5.414l4.293 4.293a1 1 0 010 1.414z" clipRule="evenodd" />
              </svg>
            </button>
            <img src="https://picsum.photos/200/300?random=1" alt="Friends group" className="h-8 w-8 rounded-full object-cover mr-3" />
            <div>
              <h2 className="text-sm font-medium text-gray-900">Friends Group</h2>
              <p className="text-xs text-gray-500">5 members</p>
            </div>
          </div>
          <div className="flex space-x-2">
            <button className="p-1 rounded-full hover:bg-gray-100 focus:outline-none focus:ring-2 focus:ring-blue-500">
              <svg xmlns="http://www.w3.org/2000/svg" className="h-5 w-5 text-gray-600" viewBox="0 0 20 20" fill="currentColor">
                <path d="M10 12a2 2 0 100-4 2 2 0 000 4z" />
                <path fillRule="evenodd" d="M.458 10C1.732 5.943 5.522 3 10 3s8.268 2.943 9.542 7c-1.274 4.057-5.064 7-9.542 7S1.732 14.057.458 10zM14 10a4 4 0 11-8 0 4 4 0 018 0z" clipRule="evenodd" />
              </svg>
            </button>
            <button className="p-1 rounded-full hover:bg-gray-100 focus:outline-none focus:ring-2 focus:ring-blue-500">
              <svg xmlns="http://www.w3.org/2000/svg" className="h-5 w-5 text-gray-600" viewBox="0 0 20 20" fill="currentColor">
                <path fillRule="evenodd" d="M10 9a3 3 0 100-6 3 3 0 000 6zm-7 9a7 7 0 1114 0H3z" clipRule="evenodd" />
              </svg>
            </button>
          </div>
        </div>

        {/* Messages container */}
        <div className="flex-1 overflow-y-auto p-4 space-y-4 bg-gradient-to-br from-blue-50 to-indigo-100">
          {messages.map((message) => (
            <div 
              key={message.id} 
              className={`flex ${message.sender === 'You' ? 'justify-end' : 'justify-start'}`}
            >
              <div 
                className={`max-w-xs md:max-w-md px-4 py-2 rounded-lg ${
                  message.sender === 'You' 
                    ? 'bg-blue-500 text-white rounded-br-none' 
                    : 'bg-white text-gray-800 rounded-bl-none shadow'
                }`}
              >
                {message.sender !== 'You' && (
                  <p className="text-xs font-semibold text-blue-600 mb-1">{message.sender}</p>
                )}
                <p className="break-words">{message.text}</p>
                <p className={`text-xs mt-1 ${message.sender === 'You' ? 'text-blue-100' : 'text-gray-500'}`}>
                  {message.time}
                </p>
              </div>
            </div>
          ))}
          <div ref={messagesEndRef} />
        </div>

        {/* Message input */}
        <form onSubmit={handleSendMessage} className="bg-white border-t border-gray-200 p-3 flex items-center gap-2">
          <button 
            type="button"
            className="p-2 text-gray-500 hover:text-blue-500 focus:outline-none"
            aria-label="Add attachment"
          >
            <svg xmlns="http://www.w3.org/2000/svg" className="h-5 w-5" viewBox="0 0 20 20" fill="currentColor">
              <path fillRule="evenodd" d="M4 3a2 2 0 00-2 2v10a2 2 0 002 2h12a2 2 0 002-2V5a2 2 0 00-2-2H4zm12 12H4l4-8 3 6 2-4 3 6z" clipRule="evenodd" />
            </svg>
          </button>
          
          <input
            ref={inputRef}
            type="text"
            value={newMessage}
            onChange={(e) => setNewMessage(e.target.value)}
            placeholder="Type a message..."
            className="flex-1 p-2 border border-gray-300 rounded-full focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-transparent"
          />
          
          <button 
            type="submit"
            className="p-2 bg-blue-500 text-white rounded-full hover:bg-blue-600 focus:outline-none focus:ring-2 focus:ring-blue-500 transition-colors duration-200"
            aria-label="Send message"
          >
            <svg xmlns="http://www.w3.org/2000/svg" className="h-5 w-5" viewBox="0 0 20 20" fill="currentColor">
              <path d="M10.894 2.553a1 1 0 00-1.788 0l-7 14a1 1 0 001.169 1.409l5-1.429A1 1 0 009 15.571V11a1 1 0 112 0v4.571a1 1 0 00.725.962l5 1.428a1 1 0 001.17-1.408l-7-14z" />
            </svg>
          </button>
        </form>
      </div>
    );
  };

  const renderContacts = () => {
    const contacts = [
      { id: 1, name: 'Alice Johnson', status: 'active', unread: 2 },
      { id: 2, name: 'Bob Smith', status: 'away', unread: 0 },
      { id: 3, name: 'Charlie Brown', status: 'active', unread: 5 },
      { id: 4, name: 'Diana Prince', status: 'offline', unread: 0 },
      { id: 5, name: 'Ethan Hunt', status: 'active', unread: 1 },
      { id: 6, name: 'Fiona Gallagher', status: 'away', unread: 0 },
    ];

    return (
      <div className="flex flex-col h-full">
        {/* Contacts header */}
        <div className="bg-white border-b border-gray-200 px-4 py-3 shadow-sm flex items-center justify-between">
          <h2 className="text-lg font-bold text-gray-800">Chats</h2>
          <div className="flex space-x-2">
            <button className="p-1 rounded-full hover:bg-gray-100 focus:outline-none">
              <svg xmlns="http://www.w3.org/2000/svg" className="h-5 w-5 text-gray-600" viewBox="0 0 20 20" fill="currentColor">
                <path d="M8 9a3 3 0 100-6 3 3 0 000 6zM8 11a6 6 0 016 6H2a6 6 0 016-6zM16 7a1 1 0 10-2 0v1h-1a1 1 0 100 2h1v1a1 1 0 102 0v-1h1a1 1 0 100-2h-1V7z" />
              </svg>
            </button>
            <button className="p-1 rounded-full hover:bg-gray-100 focus:outline-none">
              <svg xmlns="http://www.w3.org/2000/svg" className="h-5 w-5 text-gray-600" viewBox="0 0 20 20" fill="currentColor">
                <path fillRule="evenodd" d="M10 5a1 1 0 011 1v3h3a1 1 0 110 2h-3v3a1 1 0 11-2 0v-3H6a1 1 0 110-2h3V6a1 1 0 011-1z" clipRule="evenodd" />
              </svg>
            </button>
          </div>
        </div>

        {/* Search */}
        <div className="p-3 bg-gray-50 border-b border-gray-200">
          <div className="relative">
            <div className="absolute inset-y-0 left-0 pl-3 flex items-center pointer-events-none">
              <svg xmlns="http://www.w3.org/2000/svg" className="h-5 w-5 text-gray-400" viewBox="0 0 20 20" fill="currentColor">
                <path fillRule="evenodd" d="M8 4a4 4 0 100 8 4 4 0 000-8zM2 8a6 6 0 1110.89 3.476l4.817 4.817a1 1 0 01-1.414 1.414l-4.816-4.816A6 6 0 012 8z" clipRule="evenodd" />
              </svg>
            </div>
            <input
              type="text"
              placeholder="Search chats..."
              className="pl-10 pr-4 py-2 w-full rounded-full border border-gray-300 focus:outline-none focus:ring-2 focus:ring-blue-500"
            />
          </div>
        </div>

        {/* Contacts list */}
        <div className="flex-1 overflow-y-auto p-2">
          {contacts.map(contact => (
            <div 
              key={contact.id}
              onClick={() => setCurrentChat(contact.name.toLowerCase())}
              className="flex items-center p-3 mb-1 rounded-lg hover:bg-gray-100 cursor-pointer transition-colors duration-150"
            >
              <div className="relative">
                <img 
                  src={`https://picsum.photos/200/300?random=${contact.id}`} 
                  alt={contact.name}
                  className="h-10 w-10 rounded-full object-cover"
                />
                <span className={`absolute bottom-0 right-0 block h-2.5 w-2.5 rounded-full ring-2 ring-white ${
                  contact.status === 'active' ? 'bg-green-500' : 
                  contact.status === 'away' ? 'bg-yellow-500' : 
                  'bg-gray-400'
                }`}></span>
              </div>
              
              <div className="ml-3 flex-1 min-w-0">
                <div className="flex items-center justify-between">
                  <p className="font-medium text-gray-800 truncate">{contact.name}</p>
                  <p className="text-xs text-gray-500 ml-2">10:00 AM</p>
                </div>
                <p className="text-sm text-gray-500 truncate">Last message preview...</p>
              </div>
              
              {contact.unread > 0 && (
                <div className="flex-shrink-0 ml-2">
                  <span className="inline-flex items-center justify-center h-5 w-5 rounded-full bg-red-500 text-white text-xs font-bold">
                    {contact.unread}
                  </span>
                </div>
              )}
            </div>
          ))}
        </div>
      </div>
    );
  };

  return (
    <div className="min-h-screen bg-gray-100 flex">
      {/* Mobile sidebar */}
      <div className={`md:hidden fixed inset-0 z-20 transition-transform transform ${currentChat ? 'translate-x-full' : 'translate-x-0'}`}>
        <div className="absolute inset-0 bg-black opacity-50" onClick={() => setCurrentChat('')}></div>
        <div className="relative bg-white w-64 h-full shadow-xl overflow-hidden">
          {renderContacts()}
        </div>
      </div>

      {/* Desktop sidebar */}
      <div className="hidden md:block w-64 bg-white shadow-md h-screen overflow-hidden">
        {renderContacts()}
      </div>

      {/* Main chat area */}
      <div className="flex-1 flex flex-col">
        {renderChat()}
      </div>
    </div>
  );
}
```
