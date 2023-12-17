import vk_api
from vk_api.longpoll import VkLongPoll, VkEventType

# Функция обработки запросов о планах развития региона
def handle_region_plan_request(user_id, message):
    if 'планы' in message or 'развитие' in message:
        # Здесь можно добавить логику обработки запроса о планах развития региона
        # Например, формирование ответа на основе информации из базы данных
        response = 'В настоящий момент мы активно работаем над формированием планов развития региона.'
        # Прикрепление нормативной документации или ссылок
        attachment = 'document12345_67890'  # Пример прикрепленного документа (ID документа)
        send_message(user_id, response, attachment)

# Функция обработки запросов о состоянии дорог
def handle_road_status_request(user_id, message):
    if 'дороги' in message or 'ремонт' in message:
        # Логика обработки запроса о состоянии дорог
        # Например, формирование ответа и прикрепление карты или информации о ремонтах
        response = 'Информация о состоянии дорог: ...'  # Здесь будет текст с информацией
        attachment = 'map12345_67890'  # Пример прикрепленной карты (ID карты)
        send_message(user_id, response, attachment)

# Функция обработки запросов о бездомных животных
def handle_animal_shelter_request(user_id, message):
    if 'животные' in message or 'бездомные' in message:
        # Логика обработки запроса о бездомных животных
        # Формирование ответа и прикрепление информации о приютах или специализированных службах
        response = 'Информация о приютах для бездомных животных: ...'  # Текст с информацией о приютах
        attachment = 'photo12345_67890'  # Пример прикрепленной фотографии (ID фото)
        send_message(user_id, response, attachment)

# Функция отправки сообщения с возможностью прикрепления
def send_message(user_id, message, attachment=None):
    vk.messages.send(
        user_id=user_id,
        message=message,
        attachment=attachment,
        random_id=0
    )

# Основной цикл обработки событий
def main():
    # Авторизация в API VK
    token = 'YOUR_ACCESS_TOKEN'
    vk_session = vk_api.VkApi(token=token)
    vk = vk_session.get_api()
    longpoll = VkLongPoll(vk_session)

    for event in longpoll.listen():
        if event.type == VkEventType.MESSAGE_NEW and event.to_me and event.text:
            user_id = event.user_id
            message = event.text.lower()
            
            if 'привет' in message:
                send_message(user_id, 'Привет! Чем могу помочь?')
            elif 'как дела' in message:
                send_message(user_id, 'У меня все отлично, спасибо! А у вас?')
            else:
                handle_region_plan_request(user_id, message)
                handle_road_status_request(user_id, message)
                handle_animal_shelter_request(user_id, message)
                # Добавьте обработку других запросов или функций здесь

if __name__ == "__main__":
    main()
