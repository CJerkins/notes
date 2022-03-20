```
variable "default_tags" {
  description = "Default tags"
  type        = map(any)
  default = {
    Department = "Monkey"
    PoC        = "LiveStream"
  }
}

variable "storageacct_kind" {
  description = "Storage Account Kind"
  type        = string
  default     = "StorageV2"
}