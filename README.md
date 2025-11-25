<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title data-lang-key="title">AKsit EdTech AI Платформа</title>
    <!-- Подключение Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Настройка шрифта Inter для современного вида -->
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@100..900&display=swap');
        body {
            font-family: 'Inter', sans-serif;
            /* Основной темный фон */
            background-color: #0d0a13;
            color: #e5e7eb; /* Светлый текст */
        }
        /* Глобальный стиль для фокуса и акцентов */
        .ai-glow {
            box-shadow: 0 0 10px rgba(139, 92, 246, 0.5), 0 0 20px rgba(139, 92, 246, 0.3);
            transition: all 0.3s ease;
        }
        .ai-glow:hover {
            box-shadow: 0 0 15px rgba(139, 92, 246, 0.8), 0 0 30px rgba(139, 92, 246, 0.5);
        }
        .ai-border {
            border-color: #6d28d9; /* Фиолетовый акцент для границ */
        }
        /* Стиль для активной вкладки */
        .tab-active {
            background-color: #4c1d95; /* Более темный фиолетовый */
            color: #f3e8ff;
            border-bottom: 2px solid #8b5cf6;
        }
    </style>
</head>
<body class="min-h-screen">

    <!-- Основной контейнер приложения -->
    <div id="app" class="p-4 md:p-8">

        <!-- Header и Навигация -->
        <header class="flex flex-col md:flex-row justify-between items-center py-4 px-6 mb-8 rounded-xl bg-gray-900 border ai-border">
            <div class="text-2xl font-bold text-violet-400">AKsit <span class="text-white">AI EdTech</span></div>
            <div class="flex items-center space-x-4 mt-4 md:mt-0">
                <!-- Переключатель языка -->
                <button id="lang-toggle" onclick="toggleLanguage()" class="p-2 rounded-lg bg-indigo-700 text-white hover:bg-indigo-600 transition ai-glow text-sm font-semibold">
                    KZ / RU
                </button>
                <!-- Кнопка выхода -->
                <button id="logout-btn" onclick="logout()" class="p-2 rounded-lg bg-red-700 text-white hover:bg-red-600 transition text-sm font-semibold hidden">
                    <span data-lang-key="logout">Выход</span>
                </button>
            </div>
        </header>

        <!-- Секция Авторизации -->
        <section id="auth-section" class="max-w-md mx-auto p-8 rounded-2xl bg-gray-900 ai-glow border ai-border">
            <h2 class="text-3xl font-extrabold mb-6 text-center text-violet-400" data-lang-key="login_title">Вход в систему</h2>
            <form id="login-form" onsubmit="login(event)">
                <div class="mb-4">
                    <label for="fullName" class="block text-sm font-medium mb-2" data-lang-key="full_name_label">ФИО:</label>
                    <input type="text" id="fullName" class="w-full p-3 rounded-lg bg-gray-800 border border-gray-700 focus:border-violet-500 focus:ring-1 focus:ring-violet-500" required>
                </div>
                <div class="mb-6">
                    <label for="identifier" class="block text-sm font-medium mb-2" data-lang-key="identifier_label">Группа (Студент) / Предмет (Преподаватель):</label>
                    <input type="text" id="identifier" class="w-full p-3 rounded-lg bg-gray-800 border border-gray-700 focus:border-violet-500 focus:ring-1 focus:ring-violet-500" required>
                </div>
                <div class="mb-6">
                    <label class="block text-sm font-medium mb-2" data-lang-key="user_type_label">Я:</label>
                    <div class="flex space-x-4">
                        <label class="flex items-center">
                            <input type="radio" name="userType" value="student" checked class="form-radio text-violet-600">
                            <span class="ml-2 text-sm" data-lang-key="student">Студент</span>
                        </label>
                        <label class="flex items-center">
                            <input type="radio" name="userType" value="teacher" class="form-radio text-violet-600">
                            <span class="ml-2 text-sm" data-lang-key="teacher">Преподаватель</span>
                        </label>
                    </div>
                </div>
                <button type="submit" class="w-full p-3 rounded-lg bg-violet-600 hover:bg-violet-500 transition ai-glow font-semibold" data-lang-key="login_button">Войти</button>
                <p id="login-error" class="text-red-400 text-center mt-4 hidden" data-lang-key="login_error_msg">Ошибка авторизации. Проверьте данные.</p>
            </form>
        </section>

        <!-- Секция Дашборда (Скрыта по умолчанию) -->
        <section id="dashboard-section" class="hidden">
            <!-- Панель вкладок для навигации -->
            <div id="nav-tabs" class="flex flex-wrap justify-center space-x-1 sm:space-x-4 mb-8">
                <!-- Вкладки будут добавлены динамически -->
            </div>

            <!-- Контейнер для содержимого вкладок -->
            <div id="tab-content" class="p-4 md:p-8 rounded-2xl bg-gray-900 ai-glow border ai-border">
                <!-- 1. AI Генератор вопросов и материалов -->
                <div id="ai-generator" class="feature-tab">
                    <h3 class="text-2xl font-semibold mb-6 text-violet-400" data-lang-key="ai_generator_title">AI Генератор Вопросов и Материалов</h3>
                    <div class="mb-4">
                        <textarea id="ai-prompt" rows="4" class="w-full p-3 rounded-lg bg-gray-800 border border-gray-700 focus:border-violet-500" placeholder="Задайте вопрос или запросите учебный материал..." data-lang-key="ai_prompt_placeholder"></textarea>
                    </div>
                    <button onclick="generateAIContent()" id="ai-generate-btn" class="p-3 rounded-lg bg-violet-600 hover:bg-violet-500 transition ai-glow font-semibold" data-lang-key="generate_button">Сгенерировать</button>

                    <div id="ai-loading" class="mt-4 flex items-center hidden">
                        <div class="animate-spin rounded-full h-5 w-5 border-b-2 border-violet-400 mr-3"></div>
                        <span data-lang-key="loading_ai">AI генерирует ответ...</span>
                    </div>

                    <div id="ai-output" class="mt-6 p-4 rounded-lg bg-gray-800 border border-violet-500/50 min-h-24 max-h-96 overflow-y-auto whitespace-pre-wrap">
                        <p class="text-gray-400" data-lang-key="ai_output_default">Здесь появится ответ от ИИ.</p>
                    </div>
                    <div class="mt-4 text-xs text-gray-500" data-lang-key="ai_note">Примечание: Эта функция использует модель Gemini для генерации содержимого.</div>
                </div>

                <!-- 2. Автоматизация расписания (Отображается для всех) -->
                <div id="schedule-automation" class="feature-tab hidden">
                    <h3 class="text-2xl font-semibold mb-6 text-violet-400" data-lang-key="schedule_title">Автоматизированное Расписание</h3>
                    <div id="schedule-content" class="overflow-x-auto">
                        <!-- Сюда будет загружено расписание -->
                    </div>
                    <p class="text-sm mt-4 text-gray-400" data-lang-key="schedule_note">Расписание генерируется автоматически с учетом нагрузки преподавателей и расписания групп.</p>
                </div>

                <!-- 3. Система оценок и прогресса (Студент: Просмотр; Преподаватель: Управление) -->
                <div id="grades-system" class="feature-tab hidden">
                    <h3 class="text-2xl font-semibold mb-6 text-violet-400" data-lang-key="grades_title">Система Оценок и Прогресса</h3>
                    <div id="grades-content" class="overflow-x-auto">
                        <!-- Контент оценок для студента или преподавателя -->
                    </div>
                    <!-- Дополнительная функциональность для преподавателя: Проверка работ/Создание тестов (Платный тариф) -->
                    <div id="teacher-tools" class="mt-8 border-t border-gray-700 pt-6 hidden">
                        <h4 class="text-xl font-semibold mb-4 text-violet-300" data-lang-key="teacher_tools_title">Инструменты Преподавателя (Платный доступ)</h4>
                        <button onclick="showModal('test-modal')" class="p-2 rounded-lg bg-indigo-600 hover:bg-indigo-500 transition ai-glow font-semibold mr-3" data-lang-key="create_test_button">Создать Тест</button>
                        <button onclick="alert('Проверка работ: Выберите группу и предмет для проверки. (Симуляция)')" class="p-2 rounded-lg bg-indigo-600 hover:bg-indigo-500 transition ai-glow font-semibold" data-lang-key="check_papers_button">Проверить Работы</button>
                    </div>
                </div>

                <!-- 4. Рейтинг студентов (Отображается для всех) -->
                <div id="student-rating" class="feature-tab hidden">
                    <h3 class="text-2xl font-semibold mb-6 text-violet-400" data-lang-key="rating_title">Рейтинг Лучших Студентов</h3>
                    <div id="rating-content" class="bg-gray-800 p-4 rounded-xl">
                        <!-- Таблица рейтинга -->
                    </div>
                    <p class="text-sm mt-4 text-gray-400" data-lang-key="rating_note">Рейтинг основан на средних оценках за последнюю неделю/месяц.</p>
                </div>

                <!-- 5. Анкеты для студентов (Студент: Заполнение) -->
                <div id="student-forms" class="feature-tab hidden">
                    <h3 class="text-2xl font-semibold mb-6 text-violet-400" data-lang-key="forms_title">Анкеты и Запись в Секции</h3>
                    <div id="forms-list" class="space-y-4">
                        <!-- Список анкет -->
                    </div>
                </div>
                
                <!-- 6. Видеоуроки (Платный тариф / Преподаватель) -->
                <div id="video-lessons" class="feature-tab hidden">
                    <h3 class="text-2xl font-semibold mb-6 text-violet-400" data-lang-key="video_lessons_title">Видеоуроки</h3>
                    <div id="video-content" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
                         <!-- Видеокарточки -->
                    </div>
                    <div id="video-upload-section" class="mt-8 border-t border-gray-700 pt-6 hidden">
                        <h4 class="text-xl font-semibold mb-4 text-violet-300" data-lang-key="upload_video_title">Загрузить Видеоурок</h4>
                        <input type="text" placeholder="Название видео" class="w-full p-3 mb-3 rounded-lg bg-gray-800 border border-gray-700 focus:border-violet-500" data-lang-key="video_name_placeholder">
                        <input type="text" placeholder="URL-адрес видео (YouTube/Vimeo)" class="w-full p-3 mb-3 rounded-lg bg-gray-800 border border-gray-700 focus:border-violet-500" data-lang-key="video_url_placeholder">
                        <button onclick="alert('Видеоурок добавлен (Симуляция)')" class="p-3 rounded-lg bg-green-600 hover:bg-green-500 transition font-semibold" data-lang-key="add_video_button">Добавить</button>
                    </div>
                </div>

                 <!-- 7. Подписка и Тарифы -->
                 <div id="subscription-plan" class="feature-tab hidden">
                    <h3 class="text-2xl font-semibold mb-6 text-violet-400" data-lang-key="subscription_title">Планы Подписки и Тарифы</h3>
                    <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
                        <!-- Бесплатный Тариф -->
                        <div class="p-6 rounded-xl bg-gray-800 border border-violet-500/30">
                            <h4 class="text-xl font-bold mb-2 text-violet-300" data-lang-key="free_plan_name">Базовый (Бесплатно)</h4>
                            <p class="mb-4 text-gray-400" data-lang-key="free_plan_desc">Для всех студентов и преподавателей.</p>
                            <ul class="space-y-2 text-sm">
                                <li class="flex items-center"><span class="text-green-400 mr-2">✓</span> <span data-lang-key="feature_ai_basic">Доступ к AI Генератору (базовый)</span></li>
                                <li class="flex items-center"><span class="text-green-400 mr-2">✓</span> <span data-lang-key="feature_schedule">Автоматизированное расписание</span></li>
                                <li class="flex items-center"><span class="text-green-400 mr-2">✓</span> <span data-lang-key="feature_rating">Рейтинг студентов (Просмотр)</span></li>
                            </ul>
                            <button class="w-full mt-6 p-3 rounded-lg bg-gray-700 font-semibold cursor-default" data-lang-key="current_plan_button">Текущий план</button>
                        </div>

                        <!-- Преподавательский Платный Тариф -->
                        <div class="p-6 rounded-xl bg-violet-900 border-2 border-violet-500 ai-glow">
                            <h4 class="text-xl font-bold mb-2 text-white" data-lang-key="paid_teacher_name">Премиум (Преподаватель)</h4>
                            <p class="text-3xl font-extrabold text-white mb-4">1500 ₸<span class="text-base font-normal">/мес</span></p>
                            <p class="mb-4 text-violet-200" data-lang-key="paid_teacher_desc">Расширенные инструменты для оптимизации учебного процесса.</p>
                            <ul class="space-y-2 text-sm">
                                <li class="flex items-center"><span class="text-green-400 mr-2">✓</span> <span data-lang-key="feature_all_free">Все функции Базового</span></li>
                                <li class="flex items-center"><span class="text-green-400 mr-2">✓</span> <span data-lang-key="feature_teacher_tools">Система подсказок и автопроверки</span></li>
                                <li class="flex items-center"><span class="text-green-400 mr-2">✓</span> <span data-lang-key="feature_ai_advanced">Генерация учебных материалов AI</span></li>
                                <li class="flex items-center"><span class="text-green-400 mr-2">✓</span> <span data-lang-key="feature_upload_video">Загрузка видеоуроков</span></li>
                            </ul>
                            <button onclick="alert('Оплата прошла успешно (Симуляция).')" class="w-full mt-6 p-3 rounded-lg bg-yellow-500 hover:bg-yellow-400 transition font-semibold text-gray-900" data-lang-key="subscribe_button">Подписаться</button>
                        </div>
                        
                        <!-- Корпоративный Тариф (Расширение) -->
                        <div class="p-6 rounded-xl bg-gray-800 border border-violet-500/30">
                            <h4 class="text-xl font-bold mb-2 text-violet-300" data-lang-key="corporate_plan_name">Корпоративный (Расширение)</h4>
                            <p class="mb-4 text-gray-400" data-lang-key="corporate_plan_desc">Для административных групп и больших объемов.</p>
                            <ul class="space-y-2 text-sm">
                                <li class="flex items-center"><span class="text-green-400 mr-2">✓</span> <span data-lang-key="feature_full_access">Полный доступ ко всему функционалу</span></li>
                                <li class="flex items-center"><span class="text-green-400 mr-2">✓</span> <span data-lang-key="feature_analytics">Расширенная аналитика</span></li>
                                <li class="flex items-center"><span class="text-green-400 mr-2">✓</span> <span data-lang-key="feature_support">Приоритетная поддержка</span></li>
                            </ul>
                            <button onclick="alert('Свяжитесь с нами для обсуждения тарифа (Симуляция).')" class="w-full mt-6 p-3 rounded-lg bg-violet-600 hover:bg-violet-500 transition font-semibold" data-lang-key="contact_button">Связаться</button>
                        </div>
                    </div>
                </div>
            </div>
        </section>

    </div>

    <!-- Модальное окно для создания теста (Teacher Tool) -->
    <div id="test-modal" class="fixed inset-0 bg-black bg-opacity-75 flex items-center justify-center z-50 hidden">
        <div class="bg-gray-900 p-8 rounded-xl w-full max-w-lg mx-4 ai-glow border ai-border">
            <h3 class="text-2xl font-semibold mb-6 text-violet-400" data-lang-key="modal_create_test_title">Создание Теста</h3>
            <form>
                <div class="mb-4">
                    <label for="test-name" class="block text-sm font-medium mb-2" data-lang-key="test_name_label">Название Теста:</label>
                    <input type="text" id="test-name" class="w-full p-3 rounded-lg bg-gray-800 border border-gray-700 focus:border-violet-500" required>
                </div>
                <div class="mb-6">
                    <label for="test-questions" class="block text-sm font-medium mb-2" data-lang-key="test_questions_label">Вопросы (каждый с новой строки):</label>
                    <textarea id="test-questions" rows="6" class="w-full p-3 rounded-lg bg-gray-800 border border-gray-700 focus:border-violet-500" data-lang-key="test_questions_placeholder" placeholder="Вопрос 1: Что такое ИИ?&#10;Вопрос 2: Формула Эйнштейна?"></textarea>
                </div>
                <div class="flex justify-end space-x-4">
                    <button type="button" onclick="hideModal('test-modal')" class="p-2 rounded-lg bg-gray-700 hover:bg-gray-600 font-semibold" data-lang-key="cancel_button">Отмена</button>
                    <button type="submit" onclick="alert('Тест создан и готов к назначению (Симуляция).')" class="p-2 rounded-lg bg-violet-600 hover:bg-violet-500 font-semibold" data-lang-key="save_button">Сохранить</button>
                </div>
            </form>
        </div>
    </div>


    <script>
        // Глобальная переменная для текущего языка
        let currentLang = 'ru';

        // Объект с двуязычными данными
        const langData = {
            'ru': {
                'title': 'AKsit EdTech AI Платформа',
                'logout': 'Выход',
                'login_title': 'Вход в систему',
                'full_name_label': 'ФИО:',
                'identifier_label': 'Группа (Студент) / Предмет (Преподаватель):',
                'user_type_label': 'Я:',
                'student': 'Студент',
                'teacher': 'Преподаватель',
                'login_button': 'Войти',
                'login_error_msg': 'Ошибка авторизации. Проверьте данные.',
                'tab_ai': 'AI Генератор',
                'tab_schedule': 'Расписание',
                'tab_grades': 'Оценки',
                'tab_rating': 'Рейтинг',
                'tab_forms': 'Анкеты',
                'tab_videos': 'Видеоуроки',
                'tab_subscription': 'Подписка',
                'ai_generator_title': 'AI Генератор Вопросов и Материалов',
                'ai_prompt_placeholder': 'Задайте вопрос или запросите учебный материал...',
                'generate_button': 'Сгенерировать',
                'loading_ai': 'AI генерирует ответ...',
                'ai_output_default': 'Здесь появится ответ от ИИ.',
                'ai_note': 'Примечание: Эта функция использует модель Gemini для генерации содержимого.',
                'schedule_title': 'Автоматизированное Расписание',
                'schedule_note': 'Расписание генерируется автоматически с учетом нагрузки преподавателей и расписания групп.',
                'grades_title': 'Система Оценок и Прогресса',
                'teacher_tools_title': 'Инструменты Преподавателя (Платный доступ)',
                'create_test_button': 'Создать Тест',
                'check_papers_button': 'Проверить Работы',
                'rating_title': 'Рейтинг Лучших Студентов',
                'rating_note': 'Рейтинг основан на средних оценках за последнюю неделю/месяц.',
                'forms_title': 'Анкеты и Запись в Секции',
                'video_lessons_title': 'Видеоуроки',
                'upload_video_title': 'Загрузить Видеоурок',
                'video_name_placeholder': 'Название видео',
                'video_url_placeholder': 'URL-адрес видео (YouTube/Vimeo)',
                'add_video_button': 'Добавить',
                'subscription_title': 'Планы Подписки и Тарифы',
                'free_plan_name': 'Базовый (Бесплатно)',
                'free_plan_desc': 'Для всех студентов и преподавателей.',
                'feature_ai_basic': 'Доступ к AI Генератору (базовый)',
                'feature_schedule': 'Автоматизированное расписание',
                'feature_rating': 'Рейтинг студентов (Просмотр)',
                'current_plan_button': 'Текущий план',
                'paid_teacher_name': 'Премиум (Преподаватель)',
                'paid_teacher_desc': 'Расширенные инструменты для оптимизации учебного процесса.',
                'feature_all_free': 'Все функции Базового',
                'feature_teacher_tools': 'Система подсказок и автопроверки',
                'feature_ai_advanced': 'Генерация учебных материалов AI',
                'feature_upload_video': 'Загрузка видеоуроков',
                'subscribe_button': 'Подписаться',
                'corporate_plan_name': 'Корпоративный (Расширение)',
                'corporate_plan_desc': 'Для административных групп и больших объемов.',
                'feature_full_access': 'Полный доступ ко всему функционалу',
                'feature_analytics': 'Расширенная аналитика',
                'feature_support': 'Приоритетная поддержка',
                'contact_button': 'Связаться',
                'modal_create_test_title': 'Создание Теста',
                'test_name_label': 'Название Теста:',
                'test_questions_label': 'Вопросы (каждый с новой строки):',
                'test_questions_placeholder': 'Вопрос 1: Что такое ИИ?\nВопрос 2: Формула Эйнштейна?',
                'cancel_button': 'Отмена',
                'save_button': 'Сохранить',
                // Mock Data texts
                'schedule_monday': 'Понедельник',
                'schedule_tuesday': 'Вторник',
                'schedule_math': 'Математика',
                'schedule_phys': 'Физика',
                'schedule_ai_eng': 'AI Инжиниринг',
                'grade_weekly_report': 'Еженедельный отчет по оценкам',
                'grade_subject': 'Предмет',
                'grade_week1': 'Неделя 1',
                'grade_week2': 'Неделя 2',
                'grade_average': 'Средний балл',
                'rating_student': 'Студент',
                'rating_group': 'Группа',
                'rating_score': 'Балл',
                'rating_you': '(Вы)',
                'form_sport': 'Спортивная секция',
                'form_culture': 'Культурная секция',
                'form_fill': 'Заполнить анкету',
                'video_title_ai': 'Введение в AI (Лекция 1)',
                'video_title_phys': 'Квантовая механика',
                'teacher_grade_input': 'Введите оценку (Студент: [NAME], Предмет: [SUBJECT]):',
                'teacher_grade_button': 'Поставить оценку',
            },
            'kz': {
                'title': 'AKsit EdTech AI Платформасы',
                'logout': 'Шығу',
                'login_title': 'Жүйеге кіру',
                'full_name_label': 'Аты-жөні:',
                'identifier_label': 'Топ (Студент) / Пән (Оқытушы):',
                'user_type_label': 'Мен:',
                'student': 'Студент',
                'teacher': 'Оқытушы',
                'login_button': 'Кіру',
                'login_error_msg': 'Авторизация қатесі. Деректерді тексеріңіз.',
                'tab_ai': 'ЖИ Генератор',
                'tab_schedule': 'Кесте',
                'tab_grades': 'Бағалар',
                'tab_rating': 'Рейтинг',
                'tab_forms': 'Сауалнамалар',
                'tab_videos': 'Видеосабақтар',
                'tab_subscription': 'Жазылым',
                'ai_generator_title': 'Сұрақтар мен Материалдардың ЖИ Генераторы',
                'ai_prompt_placeholder': 'Сұрағыңызды немесе оқу материалын сұраңыз...',
                'generate_button': 'Генерациялау',
                'loading_ai': 'ЖИ жауапты генерациялауда...',
                'ai_output_default': 'ЖИ жауабы осы жерде пайда болады.',
                'ai_note': 'Ескерту: Бұл функция мазмұнды генерациялау үшін Gemini моделін пайдаланады.',
                'schedule_title': 'Автоматтандырылған Кесте',
                'schedule_note': 'Кесте оқытушылардың жүктемесі мен топтардың кестесін ескере отырып автоматты түрде жасалады.',
                'grades_title': 'Бағалау және Прогресс Жүйесі',
                'teacher_tools_title': 'Оқытушы Құралдары (Ақылы қолжетімділік)',
                'create_test_button': 'Тест құру',
                'check_papers_button': 'Жұмыстарды тексеру',
                'rating_title': 'Үздік Студенттер Рейтингі',
                'rating_note': 'Рейтинг соңғы апта/айдағы орташа бағаларға негізделген.',
                'forms_title': 'Сауалнамалар және Секцияларға Жазылу',
                'video_lessons_title': 'Видеосабақтар',
                'upload_video_title': 'Видеосабақ Жүктеу',
                'video_name_placeholder': 'Видео атауы',
                'video_url_placeholder': 'Видео URL-мекенжайы (YouTube/Vimeo)',
                'add_video_button': 'Қосу',
                'subscription_title': 'Жазылым Жоспарлары мен Тарифтер',
                'free_plan_name': 'Базалық (Тегін)',
                'free_plan_desc': 'Барлық студенттер мен оқытушылар үшін.',
                'feature_ai_basic': 'ЖИ Генераторына қолжетімділік (базалық)',
                'feature_schedule': 'Автоматтандырылған кесте',
                'feature_rating': 'Студенттер рейтингі (Көру)',
                'current_plan_button': 'Ағымдағы жоспар',
                'paid_teacher_name': 'Премиум (Оқытушы)',
                'paid_teacher_desc': 'Оқу процесін оңтайландыру үшін кеңейтілген құралдар.',
                'feature_all_free': 'Базалық функциялардың барлығы',
                'feature_teacher_tools': 'Кеңес беру және автотексеру жүйесі',
                'feature_ai_advanced': 'ЖИ оқу материалдарын генерациялау',
                'feature_upload_video': 'Видеосабақтарды жүктеу',
                'subscribe_button': 'Жазылу',
                'corporate_plan_name': 'Корпоративтік (Кеңейту)',
                'corporate_plan_desc': 'Әкімшілік топтар мен үлкен көлемдер үшін.',
                'feature_full_access': 'Барлық функционалға толық қолжетімділік',
                'feature_analytics': 'Кеңейтілген аналитика',
                'feature_support': 'Басымдықты қолдау',
                'contact_button': 'Хабарласу',
                'modal_create_test_title': 'Тест Құру',
                'test_name_label': 'Тест Атауы:',
                'test_questions_label': 'Сұрақтар (әрқайсысы жаңа жолдан):',
                'test_questions_placeholder': 'Сұрақ 1: ЖИ деген не?\nСұрақ 2: Эйнштейн формуласы?',
                'cancel_button': 'Бас тарту',
                'save_button': 'Сақтау',
                // Mock Data texts
                'schedule_monday': 'Дүйсенбі',
                'schedule_tuesday': 'Сейсенбі',
                'schedule_math': 'Математика',
                'schedule_phys': 'Физика',
                'schedule_ai_eng': 'ЖИ Инжиниринг',
                'grade_weekly_report': 'Апталық бағалар есебі',
                'grade_subject': 'Пән',
                'grade_week1': '1-апта',
                'grade_week2': '2-апта',
                'grade_average': 'Орташа балл',
                'rating_student': 'Студент',
                'rating_group': 'Топ',
                'rating_score': 'Балл',
                'rating_you': '(Сіз)',
                'form_sport': 'Спорт секциясы',
                'form_culture': 'Мәдениет секциясы',
                'form_fill': 'Сауалнама толтыру',
                'video_title_ai': 'ЖИ-ға кіріспе (1-дәріс)',
                'video_title_phys': 'Кванттық механика',
                'teacher_grade_input': 'Бағаны енгізіңіз (Студент: [NAME], Пән: [SUBJECT]):',
                'teacher_grade_button': 'Баға қою',
            }
        };

        // Пользовательские данные
        let currentUser = null;
        let isTeacher = false;
        let isSubscribed = false; // Симуляция платного тарифа

        // Асинхронная функция для обновления UI в соответствии с выбранным языком
        function updateUI() {
            document.querySelectorAll('[data-lang-key]').forEach(element => {
                const key = element.getAttribute('data-lang-key');
                if (langData[currentLang] && langData[currentLang][key]) {
                    if (element.tagName === 'INPUT' && element.hasAttribute('placeholder')) {
                         element.setAttribute('placeholder', langData[currentLang][key]);
                    } else if (element.tagName === 'TEXTAREA' && element.hasAttribute('placeholder')) {
                         element.setAttribute('placeholder', langData[currentLang][key]);
                    } else {
                        element.textContent = langData[currentLang][key];
                    }
                }
            });
            // Обновляем заголовок страницы
            document.title = langData[currentLang].title;
            // Перерисовываем динамический контент
            if (currentUser) {
                 renderTabs();
                 // Проверяем, существует ли активная вкладка, чтобы избежать ошибки при смене языка
                 const activeTabId = document.querySelector('.tab-button.tab-active')?.id.replace('-tab-btn', '') || 'ai-generator';
                 showTab(activeTabId);
            }
        }

        // Переключение языка
        function toggleLanguage() {
            currentLang = currentLang === 'ru' ? 'kz' : 'ru';
            updateUI();
        }

        // Обработка авторизации
        function login(event) {
            event.preventDefault();
            const fullName = document.getElementById('fullName').value.trim();
            const identifier = document.getElementById('identifier').value.trim();
            const userType = document.querySelector('input[name="userType"]:checked').value;
            const errorMsg = document.getElementById('login-error');

            // Простая симуляция авторизации
            if (fullName && identifier) {
                currentUser = {
                    fullName,
                    identifier,
                    isTeacher: userType === 'teacher',
                    isStudent: userType === 'student',
                    isSubscribed: userType === 'teacher' // Преподаватели по умолчанию на платном тарифе (симуляция)
                };

                isTeacher = currentUser.isTeacher;
                isSubscribed = currentUser.isSubscribed;

                document.getElementById('auth-section').classList.add('hidden');
                document.getElementById('dashboard-section').classList.remove('hidden');
                document.getElementById('logout-btn').classList.remove('hidden');
                errorMsg.classList.add('hidden');

                // Инициализация дашборда
                renderTabs();
                showTab('ai-generator'); // По умолчанию показываем AI Генератор
                
                // Симуляция отправки SMS (Уведомление о регистрации)
                simulateSMSNotification(fullName);

            } else {
                errorMsg.classList.remove('hidden');
            }
        }

        // Обработка выхода
        function logout() {
            currentUser = null;
            isTeacher = false;
            isSubscribed = false;
            document.getElementById('auth-section').classList.remove('hidden');
            document.getElementById('dashboard-section').classList.add('hidden');
            document.getElementById('logout-btn').classList.add('hidden');
            // Обновляем UI, чтобы сбросить язык для неавторизованной страницы
            updateUI(); 
        }
        
        // Симуляция SMS уведомления
        function simulateSMSNotification(name) {
            const msg = currentLang === 'ru' 
                ? `Успешная авторизация, ${name}! Добро пожаловать на AKsit AI EdTech платформу.`
                : `Сәтті авторизация, ${name}! AKsit AI EdTech платформасына қош келдіңіз.`;
            // В реальном приложении здесь был бы API вызов для отправки SMS.
            console.log(`[SMS-симуляция]: Отправлено уведомление пользователю ${name}. Сообщение: ${msg}`);
        }


        // Динамическое отображение вкладок в зависимости от роли
        function renderTabs() {
            const tabsContainer = document.getElementById('nav-tabs');
            tabsContainer.innerHTML = '';
            const tabs = [
                { id: 'ai-generator', key: 'tab_ai', roles: ['student', 'teacher'] },
                { id: 'schedule-automation', key: 'tab_schedule', roles: ['student', 'teacher'] },
                { id: 'grades-system', key: 'tab_grades', roles: ['student', 'teacher'] },
                { id: 'student-rating', key: 'tab_rating', roles: ['student', 'teacher'] },
                { id: 'student-forms', key: 'tab_forms', roles: ['student'] }, // Только для студентов
                { id: 'video-lessons', key: 'tab_videos', roles: ['student', 'teacher'] },
                { id: 'subscription-plan', key: 'tab_subscription', roles: ['student', 'teacher'] },
            ];

            const userRole = currentUser.isTeacher ? 'teacher' : 'student';

            tabs.filter(tab => tab.roles.includes(userRole)).forEach((tab, index) => {
                const button = document.createElement('button');
                button.id = `${tab.id}-tab-btn`;
                button.className = `tab-button p-3 rounded-t-lg font-semibold transition hover:bg-gray-800 ${index === 0 ? 'tab-active' : 'bg-gray-900'}`;
                button.setAttribute('data-lang-key', tab.key);
                button.onclick = () => showTab(tab.id);
                // FIX: Мануально устанавливаем текст для динамических кнопок, чтобы избежать рекурсии.
                button.textContent = langData[currentLang][tab.key]; 
                
                tabsContainer.appendChild(button);
            });
            // УДАЛЕНА: updateUI(); // <-- Это вызывало бесконечную рекурсию
        }

        // Переключение между вкладками
        function showTab(tabId) {
            document.querySelectorAll('.feature-tab').forEach(tab => tab.classList.add('hidden'));
            document.getElementById(tabId).classList.remove('hidden');

            document.querySelectorAll('.tab-button').forEach(btn => btn.classList.remove('tab-active'));
            const activeBtn = document.getElementById(`${tabId}-tab-btn`);
            if (activeBtn) {
                activeBtn.classList.add('tab-active');
            }

            // Динамическое обновление контента
            switch (tabId) {
                case 'schedule-automation':
                    renderSchedule();
                    break;
                case 'grades-system':
                    renderGrades();
                    break;
                case 'student-rating':
                    renderRating();
                    break;
                case 'student-forms':
                    renderForms();
                    break;
                case 'video-lessons':
                    renderVideos();
                    break;
                default:
                    break;
            }
        }
        
        // Вспомогательные функции для модальных окон
        function showModal(id) {
            document.getElementById(id).classList.remove('hidden');
        }

        function hideModal(id) {
            document.getElementById(id).classList.add('hidden');
        }

        // --- Рендеринг динамического контента ---

        // 1. Расписание (Mock Data)
        function renderSchedule() {
            const content = document.getElementById('schedule-content');
            
            // Получаем данные для текущего языка
            const mondayText = langData[currentLang].schedule_monday;
            const tuesdayText = langData[currentLang].schedule_tuesday;
            const mathText = langData[currentLang].schedule_math;
            const physText = langData[currentLang].schedule_phys;
            const aiEngText = langData[currentLang].schedule_ai_eng;

            const data = [
                { day: mondayText, time: '09:00 - 10:30', subject: mathText, group: 'АК-201' },
                { day: mondayText, time: '10:40 - 12:10', subject: physText, group: 'АК-201' },
                { day: tuesdayText, time: '13:00 - 14:30', subject: aiEngText, group: 'АК-201' },
            ];
            
            // Фильтруем расписание по группе студента (симуляция)
            const filteredData = currentUser.isStudent 
                ? data.filter(item => item.group === currentUser.identifier)
                : data; // Для преподавателя показываем все

            let html = `<table class="min-w-full divide-y divide-gray-700 rounded-xl overflow-hidden">
                <thead class="bg-violet-900">
                    <tr>
                        <th class="p-4 text-left text-sm font-medium text-white" data-lang-key="schedule_day">${currentLang === 'ru' ? 'День' : 'Күн'}</th>
                        <th class="p-4 text-left text-sm font-medium text-white" data-lang-key="schedule_time">${currentLang === 'ru' ? 'Время' : 'Уақыт'}</th>
                        <th class="p-4 text-left text-sm font-medium text-white" data-lang-key="schedule_subject">${currentLang === 'ru' ? 'Предмет' : 'Пән'}</th>
                        <th class="p-4 text-left text-sm font-medium text-white">${currentLang === 'ru' ? 'Группа' : 'Топ'}</th>
                    </tr>
                </thead>
                <tbody class="bg-gray-800 divide-y divide-gray-700">`;

            filteredData.forEach(item => {
                html += `<tr>
                    <td class="p-4 whitespace-nowrap text-sm font-medium">${item.day}</td>
                    <td class="p-4 whitespace-nowrap text-sm">${item.time}</td>
                    <td class="p-4 whitespace-nowrap text-sm text-violet-300">${item.subject}</td>
                    <td class="p-4 whitespace-nowrap text-sm">${item.group}</td>
                </tr>`;
            });

            html += `</tbody></table>`;
            content.innerHTML = html;
        }

        // 2. Оценки (Mock Data)
        function renderGrades() {
            const gradesContent = document.getElementById('grades-content');
            const teacherTools = document.getElementById('teacher-tools');
            let html = '';

            if (currentUser.isStudent) {
                teacherTools.classList.add('hidden');
                // Студент: Просмотр оценок
                // Используем переведенные названия предметов для корректного отображения
                const mathText = langData[currentLang].schedule_math;
                const physText = langData[currentLang].schedule_phys;
                const aiEngText = langData[currentLang].schedule_ai_eng;

                const studentGrades = {};
                studentGrades[mathText] = [90, 85, 87.5];
                studentGrades[physText] = [75, 80, 77.5];
                studentGrades[aiEngText] = [95, 98, 96.5];
                
                html += `<h4 class="text-xl font-semibold mb-4 text-violet-300">${langData[currentLang].grade_weekly_report} (${currentUser.fullName}, ${currentUser.identifier})</h4>
                <table class="min-w-full divide-y divide-gray-700 rounded-xl overflow-hidden">
                    <thead class="bg-violet-900">
                        <tr>
                            <th class="p-4 text-left text-sm font-medium text-white" data-lang-key="grade_subject">${langData[currentLang].grade_subject}</th>
                            <th class="p-4 text-left text-sm font-medium text-white" data-lang-key="grade_week1">${langData[currentLang].grade_week1}</th>
                            <th class="p-4 text-left text-sm font-medium text-white" data-lang-key="grade_week2">${langData[currentLang].grade_week2}</th>
                            <th class="p-4 text-left text-sm font-medium text-white" data-lang-key="grade_average">${langData[currentLang].grade_average}</th>
                        </tr>
                    </thead>
                    <tbody class="bg-gray-800 divide-y divide-gray-700">`;
                
                for (const subject in studentGrades) {
                    const [week1, week2, avg] = studentGrades[subject];
                    html += `<tr>
                        <td class="p-4 whitespace-nowrap text-sm font-medium text-violet-300">${subject}</td>
                        <td class="p-4 whitespace-nowrap text-sm">${week1}</td>
                        <td class="p-4 whitespace-nowrap text-sm">${week2}</td>
                        <td class="p-4 whitespace-nowrap text-sm font-bold ${avg > 90 ? 'text-green-400' : 'text-yellow-400'}">${avg}</td>
                    </tr>`;
                }
                
                html += `</tbody></table>`;

            } else {
                // Преподаватель: Выбор предмета и выставление оценок
                teacherTools.classList.remove('hidden');
                
                const subject = currentUser.identifier; // Предмет преподавателя
                const students = [{name: 'А. Садыков', group: 'АК-201'}, {name: 'Ж. Касенов', group: 'АК-201'}, {name: 'М. Ибрагимова', group: 'АК-202'}];
                
                html += `<h4 class="text-xl font-semibold mb-4 text-violet-300">${langData[currentLang].grades_title} (${subject})</h4>`;
                
                students.forEach(student => {
                    const name = student.name;
                    const group = student.group;
                    const gradeButtonText = langData[currentLang].teacher_grade_button;
                    // Определяем, относится ли студент к группе, которую преподает текущий преподаватель (симуляция)
                    const canGrade = group === 'АК-201'; // Предполагаем, что АК-201 - это группа преподавателя
                    const gradePlaceholderText = canGrade ? '0-100' : (currentLang === 'ru' ? 'Не ваша группа' : 'Сіздің тобыңыз емес');

                    html += `<div class="flex items-center space-x-4 p-3 bg-gray-800 rounded-lg mb-2">
                        <span class="w-48 font-medium">${name} (${group})</span>
                        <input type="number" min="0" max="100" id="grade-${name.replace(/\s/g, '-')}" 
                            class="w-32 p-2 rounded-lg bg-gray-700 border border-gray-600 focus:border-violet-500" 
                            placeholder="${gradePlaceholderText}" ${canGrade ? '' : 'disabled'}>
                        <button onclick="alert('${name} получил(а) оценку! (Симуляция)')" 
                            class="p-2 rounded-lg bg-green-600 hover:bg-green-500 transition font-semibold text-sm ${canGrade ? 'ai-glow' : 'opacity-50 cursor-not-allowed'}" 
                            ${canGrade ? '' : 'disabled'}
                            data-lang-key="teacher_grade_button">${gradeButtonText}</button>
                    </div>`;
                });
            }

            gradesContent.innerHTML = html;
        }

        // 3. Рейтинг студентов (Mock Data)
        function renderRating() {
            const content = document.getElementById('rating-content');
            const data = [
                { name: 'А. Садыков', group: 'АК-201', score: 95.2 },
                { name: 'Ж. Касенов', group: 'АК-201', score: 91.8 },
                { name: 'И. Петров', group: 'АК-202', score: 88.5 },
                { name: 'М. Ибрагимова', group: 'АК-202', score: 85.0 },
                { name: 'С. Алиев', group: 'АК-203', score: 82.1 },
            ];
            
            // Вставляем текущего пользователя в рейтинг, если его нет (для демонстрации)
            const userIndex = data.findIndex(d => d.name === currentUser.fullName);
            if (userIndex === -1 && currentUser.isStudent) {
                // Добавляем текущего пользователя со случайным баллом для примера
                data.splice(Math.floor(Math.random() * data.length), 0, { name: currentUser.fullName, group: currentUser.identifier, score: 89.0 });
            }

            // Сортировка по баллу
            data.sort((a, b) => b.score - a.score);

            let html = `<table class="min-w-full divide-y divide-gray-700">
                <thead class="bg-violet-900">
                    <tr>
                        <th class="p-3 text-left text-sm font-medium text-white">#</th>
                        <th class="p-3 text-left text-sm font-medium text-white" data-lang-key="rating_student">${langData[currentLang].rating_student}</th>
                        <th class="p-3 text-left text-sm font-medium text-white" data-lang-key="rating_group">${langData[currentLang].rating_group}</th>
                        <th class="p-3 text-right text-sm font-medium text-white" data-lang-key="rating_score">${langData[currentLang].rating_score}</th>
                    </tr>
                </thead>
                <tbody class="bg-gray-800 divide-y divide-gray-700">`;

            data.forEach((item, index) => {
                const isCurrentUser = item.name === currentUser.fullName;
                const rowClass = isCurrentUser ? 'bg-violet-700/50 ai-glow' : 'hover:bg-gray-700';
                const rank = index + 1;

                html += `<tr class="${rowClass}">
                    <td class="p-3 whitespace-nowrap text-sm font-bold">${rank}</td>
                    <td class="p-3 whitespace-nowrap text-sm font-medium text-violet-300">${item.name} ${isCurrentUser ? `<span class="text-xs text-yellow-300" data-lang-key="rating_you">${langData[currentLang].rating_you}</span>` : ''}</td>
                    <td class="p-3 whitespace-nowrap text-sm">${item.group}</td>
                    <td class="p-3 whitespace-nowrap text-sm text-right font-bold">${item.score.toFixed(1)}</td>
                </tr>`;
            });

            html += `</tbody></table>`;
            content.innerHTML = html;
        }

        // 4. Анкеты (Mock Data)
        function renderForms() {
            const content = document.getElementById('forms-list');
            
            const forms = [
                { name_key: 'form_sport', group: 'Спортивная' },
                { name_key: 'form_culture', group: 'Культурная' }
            ];

            let html = '';
            forms.forEach(form => {
                const formName = langData[currentLang][form.name_key];
                const formFillText = langData[currentLang].form_fill;
                html += `<div class="p-4 bg-gray-800 rounded-xl flex justify-between items-center border ai-border hover:bg-gray-700 transition">
                    <span class="text-lg font-medium text-violet-300">${formName} (${form.group})</span>
                    <button onclick="alert('Анкета для ${formName} заполнена! (Симуляция)')" class="p-2 rounded-lg bg-violet-600 hover:bg-violet-500 font-semibold text-sm" data-lang-key="form_fill">${formFillText}</button>
                </div>`;
            });
            
            content.innerHTML = html;
        }

        // 5. Видеоуроки (Mock Data)
        function renderVideos() {
            const content = document.getElementById('video-content');
            const uploadSection = document.getElementById('video-upload-section');

            // Секция загрузки для преподавателей и администраторов
            if (currentUser.isTeacher || isSubscribed) { // Преподаватели имеют возможность загрузки
                uploadSection.classList.remove('hidden');
            } else {
                uploadSection.classList.add('hidden');
            }

            // Получаем переведенные названия видео
            const videoTitleAi = langData[currentLang].video_title_ai;
            const videoTitlePhys = langData[currentLang].video_title_phys;
            
            const videos = [
                { title: videoTitleAi, subject: langData[currentLang].schedule_ai_eng, url: 'https://www.youtube.com/embed/dQw4w9WgXcQ' },
                { title: videoTitlePhys, subject: langData[currentLang].schedule_phys, url: 'https://www.youtube.com/embed/A3X9w4e7M9o' },
                { title: videoTitleAi, subject: langData[currentLang].schedule_ai_eng, url: 'https://www.youtube.com/embed/8r9S0v0Wvss' },
            ];

            let html = '';
            videos.forEach(video => {
                const videoTitle = video.title;
                html += `<div class="bg-gray-800 rounded-xl overflow-hidden border ai-border">
                    <div class="aspect-video">
                        <iframe class="w-full h-full" src="${video.url.replace('watch?v=', 'embed/')}" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
                    </div>
                    <div class="p-4">
                        <h5 class="text-lg font-semibold text-violet-300">${videoTitle}</h5>
                        <p class="text-sm text-gray-400">${video.subject}</p>
                    </div>
                </div>`;
            });
            
            content.innerHTML = html;
        }

        // --- AI Генератор с интеграцией Gemini API ---

        const model = "gemini-2.5-flash-preview-09-2025";
        const apiKey = ""; 
        const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/${model}:generateContent?key=${apiKey}`;

        // Функция для реализации экспоненциального отката
        async function fetchWithBackoff(url, options, maxRetries = 5) {
            for (let i = 0; i < maxRetries; i++) {
                try {
                    const response = await fetch(url, options);
                    if (response.status === 429 && i < maxRetries - 1) {
                        const delay = Math.pow(2, i) * 1000 + Math.random() * 1000;
                        await new Promise(resolve => setTimeout(resolve, delay));
                        continue;
                    }
                    if (!response.ok) {
                        throw new Error(`API call failed with status: ${response.status}`);
                    }
                    return response;
                } catch (error) {
                    if (i === maxRetries - 1) throw error;
                    const delay = Math.pow(2, i) * 1000 + Math.random() * 1000;
                    await new Promise(resolve => setTimeout(resolve, delay));
                }
            }
        }

        async function generateAIContent() {
            const prompt = document.getElementById('ai-prompt').value.trim();
            const outputDiv = document.getElementById('ai-output');
            const loadingIndicator = document.getElementById('ai-loading');
            const generateBtn = document.getElementById('ai-generate-btn');

            if (!prompt) {
                outputDiv.textContent = currentLang === 'ru' ? "Пожалуйста, введите ваш вопрос или запрос." : "Сұрағыңызды немесе сұранысыңызды енгізіңіз.";
                outputDiv.classList.add('text-yellow-400');
                return;
            }

            loadingIndicator.classList.remove('hidden');
            generateBtn.disabled = true;
            outputDiv.textContent = '';
            outputDiv.classList.remove('text-yellow-400');
            outputDiv.classList.add('text-white');

            const systemPrompt = currentLang === 'ru' 
                ? "Вы - AI-ассистент EdTech платформы AKsit. Отвечайте на вопросы студентов и генерируйте учебные материалы в дружелюбном, но информативном академическом стиле." 
                : "Сіз - AKsit EdTech платформасының ЖИ-ассистентісіз. Студенттердің сұрақтарына жауап беріңіз және оқу материалдарын достық, бірақ ақпараттық академиялық стильде жасаңыз.";

            const payload = {
                contents: [{ parts: [{ text: prompt }] }],
                tools: [{ "google_search": {} }],
                systemInstruction: {
                    parts: [{ text: systemPrompt }]
                },
            };

            try {
                const response = await fetchWithBackoff(apiUrl, {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify(payload)
                });
                
                const result = await response.json();
                const candidate = result.candidates?.[0];

                if (candidate && candidate.content?.parts?.[0]?.text) {
                    let text = candidate.content.parts[0].text;
                    let sources = [];

                    // Извлечение источников (citations)
                    const groundingMetadata = candidate.groundingMetadata;
                    if (groundingMetadata && groundingMetadata.groundingAttributions) {
                        sources = groundingMetadata.groundingAttributions
                            .map(attribution => ({
                                uri: attribution.web?.uri,
                                title: attribution.web?.title,
                            }))
                            .filter(source => source.uri && source.title);
                    }

                    // Форматирование вывода
                    outputDiv.textContent = text;
                    if (sources.length > 0) {
                        let sourceHtml = '\n\n---\n' + (currentLang === 'ru' ? 'Источники:' : 'Дереккөздер:');
                        sources.forEach((s, index) => {
                            sourceHtml += `\n${index + 1}. [${s.title}](${s.uri})`;
                        });
                        outputDiv.textContent += sourceHtml;
                    }
                } else {
                    outputDiv.textContent = currentLang === 'ru' 
                        ? "Не удалось получить ответ от AI. Попробуйте переформулировать запрос." 
                        : "ЖИ-дан жауап алу мүмкін болмады. Сұрақты басқаша тұжырымдап көріңіз.";
                    outputDiv.classList.add('text-red-400');
                }

            } catch (error) {
                console.error("AI Generation Error:", error);
                outputDiv.textContent = currentLang === 'ru' 
                    ? `Произошла ошибка при обращении к AI: ${error.message}`
                    : `ЖИ-ға жүгіну кезінде қате орын алды: ${error.message}`;
                outputDiv.classList.add('text-red-400');
            } finally {
                loadingIndicator.classList.add('hidden');
                generateBtn.disabled = false;
            }
        }


        // Инициализация при загрузке страницы
        window.onload = function() {
            // Установка языка по умолчанию и обновление UI
            const storedLang = localStorage.getItem('ak_edtech_lang');
            if (storedLang && langData[storedLang]) {
                currentLang = storedLang;
            }
            updateUI();
        };

        // Сохранение языка при изменении (для персистентности)
        document.getElementById('lang-toggle').addEventListener('click', () => {
            localStorage.setItem('ak_edtech_lang', currentLang);
        });

    </script>
</body>
</html>
