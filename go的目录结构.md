# go.sum

用于记录项目的直接和间接依赖项的版本和哈希值
这个文件的每一行都表示一个依赖项及其版本和哈希值，其格式为 module/path/version go.mod_checksum，
其中 module/path/version 是依赖项的模块路径和版本号，go.mod_checksum 是对应的 go.mod 文件的哈希值

go.sum 文件的主要作用是确保项目的依赖项在构建和部署过程中的一致性和完整性。
当依赖项的版本或哈希值发生变化时，go.sum 文件也会相应地更新。
此外，go.sum 文件还会保留过去使用的包的版本信息，以便日后可能的版本回退
