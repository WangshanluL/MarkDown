- [ ] ​    chat_messages = await master_message_service.get_chat_messages_from_redis(chat_id=chat_id,master_id=master_user_id)

# 首先从redis获取，若没有就调用get_message_with_id从数据库获取，并读取到redis，如果这时候读取的chat_messages是新格式    msg_id和role、content、type

​    summary_history = await master_chat_service.get_summary_history_from_redis(id=chat_id)

- [ ] ​    qwen_messages = await chat_qwen_service.transform_message(chat_messages=chat_messages,summary_history=summary_history)  #把从redis获取到的历史记录加到qwenmessages里面

# 这里要改，因为是用的mastermessage格式