# Попробуйте найти библиотеку 'faac' с помощью компилятора и запишите результат.
# Аргумент 'required' определяется опцией 'faac', определенной в другом месте.
faac_dep = cc.find_library('faac', required : get_option('faac'))
message('faac_dep found: ' + faac_dep.found().to_string())
message('from faac meson build')
message('6 plugins_install_dir: ' + plugins_install_dir) 
plugins_install_dir = join_paths(get_option('prefix'), get_option('libdir'), 'gstreamer-1.0')
message('8 plugins_install_dir: ' + plugins_install_dir)
# Проверьте, существует ли заголовочный файл 'faac.h' и содержит ли он символ 'faacEncOpen'.
# Это важно для обеспечения доступности необходимых функций.
have_faac_h = cc.has_header_symbol('faac.h', 'faacEncOpen')

# Если заголовок 'faac.h' требуется, но не найден, а опция 'faac' включена,
# затем остановите процесс настройки с сообщением об ошибке.
if not have_faac_h and get_option('faac').enabled()
  error('faac plugin enabled: found libfaac, but not faac.h')
endif

# Если найдены и библиотека 'faac', и заголовок 'faac.h' с символом,
# затем перейдите к созданию целевой библиотеки для 'gstfaac'.
if faac_dep.found() and have_faac_h
  # Определите целевую библиотеку с именем 'gstfaac' и исходным файлом 'gstfaac.c'.  
# Указываются дополнительные аргументы компилятора, каталоги include и зависимости.  
# Аргумент 'install' гарантирует, что библиотека будет установлена в системе,  
# и 'install_dir' указывает каталог, в который он будет установлен.
  gstfaac = library('gstfaac',
    'gstfaac.c',
    c_args : gst_plugins_bad_args,       # Аргументы компилятора для этой цели.
    include_directories : [configinc],   # Каталоги включения для заголовочных файлов.
    dependencies : [                      # Список зависимостей, необходимых этой цели.
      gstaudio_dep,                      # Dependency on GStreamer's audio component.
      gstpbutils_dep,                    # Dependency on GStreamer's pbutils component.
      gsttag_dep,                        # Dependency on GStreamer's tag component.
      faac_dep                           # Dependency on the 'faac' library.
    ],
    install : true,                      # Укажите, что эта цель должна быть установлена.
    install_dir : plugins_install_dir,    # Каталог, в который будет установлена библиотека.
  )

  # Добавьте цель библиотеки 'gstfaac' в список плагинов GStreamer, которые будут собраны.  
  # Этот список используется в других местах конфигурации сборки для управления всеми плагинами.
  plugins += [gstfaac]
endif
